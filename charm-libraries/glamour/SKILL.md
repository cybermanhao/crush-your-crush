---
name: glamour
description: "Glamour v2 Markdown renderer for terminals. Use when rendering Markdown in terminal applications, creating rich terminal output with formatting, or converting Markdown to ANSI terminal codes. Triggers on glamour, markdown terminal, terminal markdown, ANSI markdown."
---

# Glamour v2 Development Skill

> Render Markdown in the terminal with charm.land/glamour/v2

## Installation

```bash
go get charm.land/glamour/v2
```

## Quick Start

```go
import "charm.land/glamour/v2"

markdown := "# Hello\n**World**"
r, _ := glamour.NewTermRenderer(glamour.DefaultStyles())
output, _ := r.Render(markdown)
fmt.Println(output)
```

## Renderers

### Term Renderer

For standard terminal output:

```go
r, err := glamour.NewTermRenderer(
    glamour.WithStyles(glamour.DefaultStyles()),
    glamour.WithWordWrap(80),
)
output, err := r.Render(markdown)
```

### IRC Renderer

For IRC clients:

```go
r, _ := glamour.NewTermRenderer(
    glamour.WithStyles(glamour.DefaultStyles()),
    glamour.WithChrome("chat"),
)
output, _ := r.Render(markdown)
```

### ANSI Renderer

For raw ANSI codes:

```go
r, _ := glamour.NewANSIRenderer(markdown, glamour.DefaultStyles())
output, _ := r.Render()
```

## Configuration Options

### Word Wrap

```go
// Fixed width
r, _ := glamour.NewTermRenderer(
    glamour.WithWordWrap(80),
)

// No wrap (0)
r, _ := glamour.NewTermRenderer(
    glamour.WithWordWrap(0),
)
```

### Custom Styles

```go
myStyle := glamour.StyleConfig{
    Document: glamour.StylePrimitive{
        WhiteSpace: "pre-wrap",
    },
    Heading: glamour.StylePrimitive{
        Bold: true,
        Foreground: glamour.Color("62"),
    },
    CodeBlock: glamour.StylePrimitive{
        Background: glamour.Color("236"),
    },
    Code: glamour.StylePrimitive{
        Color: glamour.Color("226"),
    },
}

r, _ := glamour.NewTermRenderer(
    glamour.WithStyles(myStyle),
)
```

###-Preset Styles

```go
// GitHub style
glamour.GitHubStyle()

// Dracula style
glamour.DraculaStyle()

// NOTTY style (no colors)
glamour.NottyStyle()
```

## Style Configuration

### Available Style Keys

```go
type StyleConfig struct {
    Document     StylePrimitive  // Root document
    Paragraph    StylePrimitive
    BlockQuote  StylePrimitive
    List        StylePrimitive
    ListItem    StylePrimitive
    Heading     StylePrimitive
    Heading1    StylePrimitive
    Heading2    StylePrimitive
    Heading3    StylePrimitive
    Heading4    StylePrimitive
    Heading5    StylePrimitive
    Heading6    StylePrimitive
    CodeBlock   StylePrimitive
    Code       StylePrimitive
    Table       StylePrimitive
    TableCell   StylePrimitive
    TableHeader StylePrimitive
    Strong      StylePrimitive
    Emph       StylePrimitive
    Link        StylePrimitive
    LinkDest   StylePrimitive
    HRule       StylePrimitive
    Item        StylePrimitive
    ItemMarker  StylePrimitive
    Strikethrough StylePrimitive
}
```

### Style Primitive Properties

```go
type StylePrimitive struct {
    // Text styling
    Color            string
    Background      string
    Bold            bool
    Italic          bool
    Strikethrough   bool
    
    // Spacing
    MarginTop       string
    MarginBottom    string
    MarginLeft      string
    MarginRight     string
    PaddingTop      string
    PaddingBottom   string
    PaddingLeft     string
    PaddingRight    string
    
    // Layout
    WhiteSpace      string  // "pre" or "pre-wrap"
    Float          string  // "left" or "right"
}
```

## Common Patterns

### Markdown Editor Preview

```go
func Preview(markdown string, width int) string {
    r, err := glamour.NewTermRenderer(
        glamour.WithStyles(glamour.DraculaStyle()),
        glamour.WithWordWrap(width),
    )
    if err != nil {
        return markdown // Fallback to plain text
    }
    output, _ := r.Render(markdown)
    return output
}
```

### Code Highlighting

```go
import "github.com/alecthomas/chroma/v2"

// Use Chroma for syntax highlighting
r, _ := glamour.NewTermRenderer(
    glamour.WithStyles(glamour.DefaultStyles()),
    glamour.WithChromaStyles(chroma.Background),
    glamour.WithWordWrap(80),
)
```

### Interactive Selection

```go
const markdown = `
# Options

1. Option A
2. Option B
3. Option C
`

r, _ := glamour.NewTermRenderer(
    glamour.WithStyles(glamour.GitHubStyle()),
)

// User can copy from terminal
fmt.Println(r.Render(markdown))
```

## Best Practices

### Error Handling

```go
r, err := glamour.NewTermRenderer(
    glamour.WithStyles(glamour.DefaultStyles()),
)
if err != nil {
    log.Fatal(err)
}
output, err := r.Render(markdown)
if err != nil {
    log.Fatal(err)
}
```

### Width Calculation

```go
import "github.com/charmbracelet/x/ansi"

// Get terminal width
_, width :=ansi.GetTermSize()
if width == 0 {
    width = 80 // Default fallback
}

r, _ := glamour.NewTermRenderer(
    glamour.WithWordWrap(width),
)
```

### Caching

```go
// Cache rendered output for static content
var markdownCache = make(map[string]string)

func RenderMarkdown(markdown string) string {
    if cached, ok := markdownCache[markdown]; ok {
        return cached
    }
    r, _ := glamour.NewTermRenderer(glamour.DefaultStyles())
    output, _ := r.Render(markdown)
    markdownCache[markdown] = output
    return output
}
```

## Related

- [Lipgloss](https://charm.land/lipgloss) - Terminal styling (use lipgloss skill)
- [Bubble Tea](https://charm.land/bubbletea) - TUI framework (use bubbletea skill)
