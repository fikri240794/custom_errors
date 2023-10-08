# Custom Errors
Go custom error struct, implement Go error interface with status code, message, and error struct fields validation (optional).

## Installation
```bash
go get github.com/fikri240794/custom_errors
```

## Usage
Example how to use Custom Error:
```go
package main

import (
	"fmt"
	"net/http"
	"playground/custom_errors"
)

func main() {
	var (
		err           error
		isCustomError bool
		customError   custom_errors.Error
	)

	// error from custom error
	err = custom_errors.New(
		http.StatusBadRequest,
		http.StatusText(http.StatusBadRequest),
		custom_errors.NewErrorField("field1", "field is required"),
		custom_errors.NewErrorField("field2", fmt.Sprintf("min value is %d", 50)),
		custom_errors.NewErrorField("fieldN", "error message validation"),
	)

	fmt.Println(err.Error()) // Bad Request

	// parse error to custom error
	// if err parameter is custom error
	// will return custom error and err is nil
	// otherwise, will return passed err parameter
	isCustomError, customError = custom_errors.Parse(err)

	if isCustomError {
		fmt.Println(customError.Code)    // 400
		fmt.Println(customError.Message) // Bad Request
		for i := 0; i < len(customError.ErrorFields); i++ {
			fmt.Println(customError.ErrorFields[i].Field)   // fieldN
			fmt.Println(customError.ErrorFields[i].Message) // error message validation
		}
	}
}
```