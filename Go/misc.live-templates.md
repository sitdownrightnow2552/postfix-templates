# Golang code templates

Useful live templates and code completion for Goland IDE.

Install `settings.zip`: https://www.jetbrains.com/help/go/sharing-live-templates.html#import

## Golang

### if err not nil return error

Checks that err is not nil and writes return statement with default return values.
Abbreviation:

```
erre
```

Template text:

```go
if $ERR$ != nil {
  return $RET$$TEXT$
}
```

Applicable in Go: statement

| Name   | Expression                                               | Default value | Skip if defined |
|--------|----------------------------------------------------------|---------------|-----------------|
| $ERR$  | `errorVariable()`                                        | "err"         |                 |
| $RET$  | `regularExpression(defaultReturnValues, "([^,]*$)", "")` |               |                 |
| $TEXT$ | `errorVariable()`                                        |               |                 |

### if err not nil panic

Checks that err is not nil and panic

Abbreviation:

```
errp
```

Template text:

```go
if $ERR$ != nil {
  return panic($TEXT$)
}
```

Applicable in Go: statement

| Name   | Expression        | Default value | Skip if defined |
|--------|-------------------|---------------|-----------------|
| $ERR$  | `errorVariable()` | "err"         |                 |
| $TEXT$ | `errorVariable()` |               |                 |

### if err not nil return wrapped

Checks that err is not nil and writes return statement with default return values, except last - error. It would be
wrapped.
`fmt.Errorf` can be easily replaced by any other function

Abbreviation:

```
errr
```

Template text:

```go
if $ERR$ != nil {
 return $RETURN$fmt.Errorf("$TEXT$: %w", $ERR$)
}
```

Applicable in Go: statement

| Name     | Expression                                               | Default value | Skip if defined |
|----------|----------------------------------------------------------|---------------|-----------------|
| $ERR$    | `errorVariable()`                                        | "err"         |                 |
| $RETURN$ | `regularExpression(defaultReturnValues, "([^,]*$)", "")` |               |                 |
| $TEXT$   | `errorVariable()`                                        |               |                 |

### Json field names in camelCase

Sometimes we need to declare json fields not in snake_case format (goland used it by default), but in camelCase.
This pattern also can be applied to any other tags, like `sql`, `pg`, `csv`.

Abbreviation:

```
jsonc
```

Template text:

```go
json:"$FIELD_NAME$"
```

Applicable in Go: tag literal

| Name         | Expression             | Default value | Skip if defined |
|--------------|------------------------|---------------|-----------------|
| $FIELD_NAME$ | camelCase(fieldName()) |               |                 |

### Safe getter

Generates getter for struct field in protoc-gen-go style.
Why do you need it?
It allows access to nested fields without worrying about nil pointers.

```go
type Value struct {
    Field1 *Struct1
}
type Struct1 struct {
    Field2 *Struct2
}
type Struct2 struct {
    UsefulString *string
}

var value *Value
var _ = value.SafeField1().SafeField2().SafeUsefulString()

func (v *Value) SafeField1() *Struct1 {
	if v == nil {
		return nil
	}
	return v.Field1
}
func (s *Struct1) SafeField2() *Struct2 {
	if s == nil {
		return nil
	}
	return s.Field2
}
func (s *Struct2) SafeUsefulString() *string {
	if s == nil {
		return nil
	}
	return s.UsefulString
}
```

Abbreviation:

```
getter
```

Template text:

```go
func ($RECEIVER$ *$TYPE_1$) Safe$NAME$() $TYPE_2$ {
	if $RECEIVER$ == nil {
		return $RESULT$$END$
	}
	return $RECEIVER$.$NAME$
}
```