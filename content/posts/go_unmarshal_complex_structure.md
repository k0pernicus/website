+++
title = "(Un)marshal complex JSON objects, in Go"
date = 2019-04-16T20:03:16+02:00
description = "Deep dive into the Hell of Unmarshalling..."
draft = false
categories = ["go"]
tags = ["go", "marshalling", "unmarshalling", "json"]
disqus = false
+++

If you work a lot with APIs in Go, it may happen you have to work with complex
JSON responses...

Ok, let's talk about that using a simple example.

Imagine you have to work with the following data structure, which represents a
`Person` data structure (like a real person, no trap) with the following
characteristics:

* the name of a Person object: a string;
* the dog's name of the Person object (if it exists): a Dog structure that
  contains only a string, which is its name;
* the age of the Person object: an integer;
* the last time the data structure has been updated: a datetime type.

First, we try to implement the associated Go structure, which is very simple:

```go
type Person struct {
  Name      string    `json:"name"`
  DogS      Dog       `json:"dog"`
  Age       int       `json:"age"`
  UpdatedAt time.Time `json:"updatedAt"`
}

type Dog struct {
  Name string `json:"name"`
}
```

For now, everything is ok, and we imagine the JSON response like

```json
{
  "name": "Ferdinand",
  "age": 25,
  "dog": {
    "name": "Rex"
  },
  updatedAt: "2019-01-01 00:00Z"
}
```

Unfortunately, the response does not look like this.
*At all*

In contrast, the data in JSON looks like this:

```json
{
  "name": "Ferdinand",
  "age": "25",
  "updatedAt": "2019-01-01 00:00"
}
```

There is (at least) three big issues in our previous Go code.
Let's decompose what (and where) are the problems, and how to solve them one by
one.

## Handle the missed data

As you may already noticed, we miss a data in the JSON response: the `dog` object.

A basic solution to this is to implement a structure *if* the dog JSON object
is present, or *if* the dog JSON object is not present...
Spoiler: this is a ***very bad*** solution.

When you are (un)marshalling a JSON object in Go, you can omit some fields *if*
the field is empty (consider empty as NULL, or just the default type value, like
an empty string for string type, or a 0 value for a default integer value).
Usually, this solution is really simple to use, because you just have to append
the `omitempty` tag in the Go struct tags you wrote before, for the field you want omit.
Using it permits also to reduce the payload if you want to send little data
structure compared to your big (and complex) Go data structure.

So, for the dog, you just have to trick your Go data model like this:

```go
type Person struct {
  Name      string    `json:"name"`
  DogS      Dog       `json:"dog,omitempty"` // We append here the 'omitempty'
  tag
  Age       int       `json:"age"`
  UpdatedAt time.Time `json:"updatedAt"`
}

type Dog struct {
  Name string `json:"name"`
}
```

## Handle the string values

Unmarshalling the previous JSON structure throws the following error: `json: cannot unmarshal
string into Go struct field Person.age of type int`.

The second issue is that the `age` field is not an integer, but a string *that
represents* an integer.
Unmarshalling this kind of thing does not work at all... without an extra tag
again!

This tag is `string`, and have to be append next the `age` field in the Go
structure:

```go
type Person struct {
  Name      string    `json:"name"`
  DogS      Dog       `json:"dog,omitempty"`
  Age       int       `json:"age,string"`
  UpdatedAt time.Time `json:"updatedAt"`
}

type Dog struct {
  Name string `json:"name"`
}
```

The `string` tag informs that the unmarshalling behaviour has to convert
explicitly the string type to the type in the Go structure.

Unfortunately, magic can disappear really quickly...
But we will explore this black magic of Go in the last section of this blog post.

## Handle the wrong datetime format

The Go `time` library [does handle multiple time
formats](https://golang.org/pkg/time/).
Unfortunately, the format of the date returned in the response does not
correspond to a supported time format, in the Go standard library.

You can try to check another Go library that handles this format (I doubt about
it...), or you can write your own time parser, for *this* specific time format:

```go
type PersonDate struct {
  time.Time
}
```

Now, we have to write our custom `MarshalJSON` and `UnmarshalJSON` functions for
this custom structure, in order to valide the interface contract with the Marshal
interface.

```go
const PersonDateFormat = "2006-01-02 15:04"

func (pd *PersonDate) UnmarshalJSON(input []byte) error {
  strInput := string(input)
  strInput = strings.Trim(strInput, `"`)
  newTime, err := time.Parse(PersonDateFormat, strInput)
  if err != nil {
    return err
  }
  pd.Time = newTime
  return nil
}

func (pd PersonDate) MarshalJSON() ([]byte, error) {
  // Here, you can return the given time in an existing
  // (and accepted) time format
  return []byte(`"` + pd.Time.Format(time.RFC1123) + `"`), nil
}
```

At this point, you can unmarshal this custom date time format in replacing the
`time.Time` type in the `Person` structure with the `PersonDate` structure, and marshal it
into another time format that is recognized by the Go library.

Nice!

## Handle the conversion errors during Unmarshalling

For this last issue, I don't think there is a "good" solution...

Imagine now that an error happened in the back side, and overrides the `age` value by an empty string
(or something other than an integer) in the JSON response.
If you have this type of response, an error will be thrown: `json: invalid use of ,string struct tag,
trying to unmarshal "" into int`.

We can't be angry with the `UnmarshalJSON` function, because this error is terribly logic.

Do not forget that unmarshalling a string into an int, using the `string` tag,
will *convert* the data type into another one.
In the case of converting a string type value into an integer, the function can explicitly call `strconv.Atoi`.
If you already used this function, you know that the function returns an
integer, or an error if the string does not represents an integer.
Like an empty string.

Unfortunately, there is no easy way to handle this error... except rewriting the
Unmarshal process for our structure, or... using two different structures.

### 1st solution: the custom Age field

Like we did for our custom Date format, we can wrap the `Age` field into a custom
Go data structure (for example, `PersonAge`), and write a custom `MarshalJSON`
and `UnmarshalJSON` methods for this Go structure.
However, if you plan in the future to create concrete type for each integer
values in the `Person` Go structure, and if you have to manage later different
integer values, you will have to create as much custom Go structure than
integer values in the JSON response...

### 2nd solution: separate the JSON structure and the final Go data structure

The purpose of this solution is to handle and unmarshal the JSON structure in a
Go structure that contains all its values as string types, and create a function
to convert this structure into another that contains the required `Person`
types, using reflection.
This solution can be generic, in order to convert all the data structures you
want into another (using `interface{}` parameters), but can be complicated if
you just want to solve this issue for this field issue *only*.

As I said previously, **there is no best advice** for this, because it depends
of the kind of response you will have in real world.
If you are sure that the JSON response will not change, and you only have one
integer (or float) field type, go for the 1st solution!
Otherwise, you can play with reflection in order to build a more concrete and
durable solution, not only for this Go structure (and JSON response) but also
for many other that share this issue.

## Go playground

If you want to play with this example, please play with [this Go
playground](https://play.golang.org/p/la3Xxw1rBCb).

