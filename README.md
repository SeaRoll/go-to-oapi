# Go types to OpenAPI Schema Generator

A simple tool to generate OpenAPI schema from Go types.

## Installation

```sh
go get github.com/SeaRoll/go-to-openapi
```

## Basic Usage

```go
package main

import (
	"github.com/SeaRoll/go-to-openapi"
)

type User struct {
	ID   int    `json:"id"`
	Name string `json:"name"`
}

type Err struct {
	Code    int    `json:"code"`
	Message string `json:"message"`
}

type GetUserEndpoint struct {
	Method string `method:"GET"`
	Path   string `path:"/users/{id}"`
	PathParams struct {
		ID int `pathparam:"id"`
	}
	QueryParams struct {
		Filter string `query:"filter"`
	}
	ResponseBody User `response:"200"`
	ErrorResponse Err `response:"400,401,500"`
}
```

## Polymorphic Types

```go
type Shape interface {}

type Circle struct {
	Type string `json:"type"`
	Radius float64 `json:"radius"`
}

type Square struct {
	Type string `json:"type"`
	Side float64 `json:"side"`
}

type ShapeResponse struct {
	Shape Shape `json:"shape" allOf:"Circle,Square" discriminator:"type"`
}

type GetShapeEndpoint struct {
	Method string `method:"GET"`
	Path   string `path:"/shapes/{id}"`
	PathParams struct {
		ID int `pathparam:"id"`
	}
	ResponseBody ShapeResponse `response:"200"`
	ErrorResponse Err `response:"400,401,500"`
}
```
