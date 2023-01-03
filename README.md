# GCStyle

**GCStyle** Go package supports you to do styled or colorized output to the console.

## Supported Styles

1. Color
1. Background Color
1. Bold
1. Italic
1. Underline
1. Strike Through
1. Darken (Darker than normal)
1. Lighten (Lighter than normal)

## Why?

1. Easy to use. Easy to integrate with existing code.
2. GCStyle uses 24-bit True color.
3. Supports CSS3 web colors.
4. Several styles of APIs are available to achieve the same output. You are free to choose any.
5. APIs have flag parameters to 'turn on' or 'turn off' styling. This provides you the opportunity to use the same code for 'console output' and 'file log'.
6. You can detect whether styling is supported by the terminal.
7. If you are developing a high-performance and low-allocation logger, GCStyle provides zero allocation `Append*` functions.

### Colors

GCStyle provides the following three methods to create colors.

1. GCStyle supports CSS named colors like Blue, Silver, Red, etc. You can view the full list of CSS named colors [here](https://www.w3.org/TR/css-color-4/#named-colors).

   ```go
   clr := wcolor.Cyan
   ```

2. You can construct colors using Red, Green, and Blue components.

   ```go
   clr := wcolor.RGB(200, 25, 200)
   ```

3. You can use CSS3 style HEX colors.

   ```go
   clr1 := wcolor.HEX("#f210fe") // 6 digit hex value with # prefix.
   clr2 := wcolor.HEX("#fff")    // 3 digit hex value with # prefix.
   ```

## Qick Start

```go
import (
    "github.com/arafath-mk/gcstyle"
    "github.com/arafath-mk/gcstyle/wcolor"
)

func main() {
    color1 := wcolor.Cyan
    color2 := wcolor.RGB(200, 25, 200)
    color3 := wcolor.HEX("#f210fe")

    // Usage 1: Apply style to a string and then print it.
    styledStr1 := gcstyle.ApplyTo("Hello World", true).Color(color1).Bold().String()
    fmt.Println(styledStr1)

    // Usage 2: Mark the start and end of the color while printing.
    fmt.Println("%sHello%s World", color2.Start(true), color2.End(true))

    // Usage 3: Define a style and apply it to a string.
    style1 := gcstyle.Style{
        Color:          wcolor.Lime.Clone()
        Background:     nil
        Bold:           true
        Italic:         false
        Underline:      false
        Strikethrough:  false
        Darken:         false
        Lighten:        false
    }
    styledStr2 := style1.ApplyTo("Hello", true).String()
    fmt.Println(styledStr2)

    // Usage 4: Mark the start and end of the style while printing.
    fmt.Println("%sHello%s World", style1.Start(true), style1.End(true))
}
```

### Usage 1: Apply styles to string

```go
import (
    "github.com/arafath-mk/gcstyle"
    "github.com/arafath-mk/gcstyle/wcolor"
)

func main() {
    // Apply Blue color to "Hello World".
    styledStr1 := gcstyle.ApplyTo("Hello World", true).Color(wcolor.Blue).String()
    fmt.Println(styledStr1)

    // You can apply other styles too. Like, Bold, Underline, etc.
    styledStr2 := gcstyle.ApplyTo("Hello World", true).Color(wcolor.Blue).Bold().Underline().String()
    fmt.Println(styledStr2)

    // You can concatinate un-styled string with styled string.
    styledStr3 := gcstyle.ApplyTo("Hello", true).Color(wcolor.Blue).String()
    styledStr3 += " World"
    // Below print statement outputs 'Hello' using Blue color. And the ' World' will be printed using the terminal's default color.
    fmt.Println(styledStr3)
}
```

### Usage 2: Mark the start and end of the color while printing

```go
import (
    "github.com/arafath-mk/gcstyle"
    "github.com/arafath-mk/gcstyle/wcolor"
)

func main() {
    color := wcolor.Cyan

    // You can mark start and end of the color while printing. Hello will be printed in Cyan.
    fmt.Println("%sHello%s World", color.Start(true), color.End(true))

    // You can mark start and end of the color while printing. World will be printed in Red.
    fmt.Println("Hello %sWorld%s", wcolor.Red.Start(true), wcolor.Red.End(true))
}
```

### Usage 3: Define style and apply it to string

```go
import (
    "github.com/arafath-mk/gcstyle"
    "github.com/arafath-mk/gcstyle/wcolor"
)

func main() {
    // Define a style and apply it to a string.
    style := gcstyle.Style{
        Color:          wcolor.Lime.Clone()
        Background:     nil
        Bold:           true
        Italic:         false
        Underline:      false
        Strikethrough:  false
        Darken:         false
        Lighten:        false
    }
    styledStr := style.ApplyTo("Hello", true).String()
    fmt.Println(styledStr)

    // You can concatinate un-styled string with styled string.
    styledStr2 := style.ApplyTo("Hello", true).String()
    styledStr2 += " World"
    // Below print statement outputs 'Hello' using Lime color with Bold style. And the ' World' will be printed using the terminal's default color and style.
    fmt.Println(styledStr2)
}
```

### Usage 4: Mark the start and end of the style while printing

```go
import (
    "github.com/arafath-mk/gcstyle"
    "github.com/arafath-mk/gcstyle/wcolor"
)

func main() {
    // Define a style.
    style := gcstyle.Style{
        Color:          wcolor.Red.Clone()
        Bold:           true
    }

    // Mark the start and end of the style while printing. 'Hello' will be printed in Red color and bold style.
    fmt.Println("%sHello%s World", style.Start(true), style.End(true))
}
```

### Conditionally Apply Styles

You can use a flag variable to conditionally apply the styles. If the value of the flag is `false`, then GCStyle will not style the output.

```go
import (
    "github.com/arafath-mk/gcstyle"
    "github.com/arafath-mk/gcstyle/wcolor"
)

func main() {
    color1 := wcolor.Cyan
    color2 := wcolor.RGB(200, 25, 200)
    color3 := wcolor.HEX("#f210fe")

    canStyle := false

    // Usage 1: Apply style to a string and then print it.
    styledStr1 := gcstyle.ApplyTo("Hello", canStyle).Color(color1).Bold().String()
    fmt.Println(styledStr1)

    // Usage 2: Mark start and end color while printing.
    fmt.Println("%sHello%s World", color2.Start(canStyle), color2.End(canStyle))

    // Usage 3: Define a style and apply it to a string.
    style1 := gcstyle.Style{
        Color:          wcolor.Lime.Clone()
        Background:     nil
        Bold:           true
        Italic:         false
        Underline:      false
        Strikethrough:  false
        Darken:         false
        Lighten:        false
    }
    styledStr2 := style1.ApplyTo("Hello", canStyle).String()
    fmt.Println(styledStr2)

    // Usage 4: Mark start and end style while printing.
    fmt.Println("%sHello%s World", style1.Start(canStyle), style1.End(canStyle))
}
```

### Check whether the terminal supports styling

You can use the `gcstyle.CanApplyStyle()` function to check whether the terminal supports styling.

```go
import (
    "github.com/arafath-mk/gcstyle"
    "github.com/arafath-mk/gcstyle/wcolor"
)

func main() {
    color1 := wcolor.Cyan
    color2 := wcolor.RGB(200, 25, 200)
    color3 := wcolor.HEX("#f210fe")

    canStyle := gcstyle.CanApplyStyle()

    // Apply style to a string and then print it.
    styledStr1 := gcstyle.ApplyTo("Hello", canStyle).Color(color1).Bold().String()
    fmt.Println(styledStr1)
}
```

## License

[The MIT License (MIT)](./LICENSE)
