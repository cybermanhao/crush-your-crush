---
name: lipgloss
description: "Lipgloss v2 terminal styling library for Go. Use when styling terminal output, creating text styles, borders, colors, or layouts for command-line applications. Triggers on lipgloss, terminal styling, CLI colors, text formatting, borders."
---

# Lipgloss v2 Development Skill

> Style your terminal applications with charm.land/lipgloss/v2

## Installation

```bash
go get charm.land/lipgloss/v2
```

## Quick Start

```go
import "charm.land/lipgloss/v2"

var style = lipgloss.NewStyle().
    Foreground(lipgloss.Color("62")).
    Bold(true)

fmt.Println(style.Render("Hello, World!"))
```

## Color System

### ANSI 256 Colors

```go
// Direct color
lipgloss.Color("62")    // 256-color palette
lipgloss.Color("#FF5733") // Hex (16.7M colors, terminal dependent)
lipgloss.Color("210")   // 256-color palette

// Terminal colors
lipgloss.Color("39")    // ANSI foreground
lipgloss.Color("255")   // ANSI background
```

### Adaptive Colors

```go
// Automatically switch based on terminal background
lipgloss.AdaptiveColor{Light: "15", Dark: "0"}

// Usage
lipgloss.NewStyle().
    Foreground(lipgloss.AdaptiveColor{Light: "2", Dark: "10"})
```

## Text Styling

### Basic Properties

```go
lipgloss.NewStyle().
    Bold(true).                // Bold text
    Faint(true).               // Faint/dim text
    Italic(true).              // Italic text
    CrossedOut(true).          // Strikethrough
    Underline(true).           // Underline
    Overline(true).            // Overline
    Blink(true).               // Blinking text
    Reverse(true).             // Reverse colors
    Invisible(true).           // Hidden text
```

### Foreground & Background

```go
lipgloss.NewStyle().
    Foreground(lipgloss.Color("62")).
    Background(lipgloss.Color("235")).
    Foreground(lipgloss.NoColor{}) // Remove foreground color
```

## Width & Alignment

### Width and Height

```go
lipgloss.NewStyle().
    Width(20).                 // Fixed width
    Height(10).                // Fixed height
    MaxWidth(50).             // Maximum width
    MaxHeight(20).            // Maximum height
```

### Alignment

```go
lipgloss.NewStyle().
    Align(lipgloss.Left).     // Default
    Align(lipgloss.Right).
    Align(lipgloss.Center).
    Align(lipgloss.Block).    // Block alignment
```

### Margins and Padding

```go
lipgloss.NewStyle().
    Margin(2).                 // All sides
    MarginTop(1).
    MarginBottom(1).
    MarginLeft(2).
    MarginRight(2).
    Padding(1).                // All sides
    PaddingTop(1).
    PaddingBottom(1).
    PaddingLeft(2).
    PaddingRight(2)
```

## Borders

### Border Styles

```go
lipgloss.NewStyle().
    Border(lipgloss.NormalBorder).
    Border(lipgloss.ThickBorder).
    Border(lipgloss.DoubleBorder).
    Border(lipgloss.RoundedBorder()).
    Border(lipgloss.HiddenBorder).
    Border(lipgloss.DashedBorder()).
    Border(lipgloss.DottedBorder())
```

### Individual Borders

```go
lipgloss.NewStyle().
    BorderStyle(lipgloss.Border{
        Top:    "═",
        Bottom: "═",
        Left:   "║",
        Right:  "║",
        TopLeft:     "╔",
        TopRight:    "╗",
        BottomLeft:  "╚",
        BottomRight: "╝",
    })
```

### Border Colors

```go
lipgloss.NewStyle().
    BorderForeground(lipgloss.Color("62")).
    BorderBackground(lipgloss.Color("235"))
```

## Layout

### Inline and Stack

```go
// Horizontal layout
lipgloss.JoinHorizontal(lipgloss.Left, style1, style2, style3)

// Vertical layout
lipgloss.JoinVertical(lipgloss.Left, style1, style2, style3)

// Both directions
lipgloss.JoinHorizontal(
    lipgloss.Top,
    lipgloss.JoinVertical(lipgloss.Left, a, b),
    lipgloss.JoinVertical(lipgloss.Left, c, d),
)
```

### Position

```go
lipgloss.Place(width, height,
    content,
    lipgloss.Center,    // Horizontal position
    lipgloss.Center,    // Vertical position
)
```

## Conditional Styling

### Terminal Detection

```go
// Check if terminal supports color
lipgloss.ColorSupported()

// Check if running in a dumb terminal
lipgloss.IsTerminal()
```

## Common Patterns

### Message Box

```go
func MessageBox(title, message string) string {
    style := lipgloss.NewStyle().
        Width(60).
        BorderStyle(lipgloss.RoundedBorder()).
        BorderForeground(lipgloss.Color("62")).
        Padding(1, 2).
        Foreground(lipgloss.Color("62"))

    return style.Render(title + "\n" + message)
}
```

### Table

```go
func Table() string {
    headers := lipgloss.NewStyle().
        Bold(true).
        Foreground(lipgloss.Color("62"))

    cell := lipgloss.NewStyle().
        Padding(0, 1)

    return lipgloss.JoinHorizontal(
        lipgloss.Top,
        headers.Width(10).Render("Name"),
        headers.Width(20).Render("Email"),
        headers.Width(15).Render("Role"),
    ) + "\n" +
        lipgloss.JoinHorizontal(
            lipgloss.Top,
            cell.Render("Alice"),
            cell.Render("alice@example.com"),
            cell.Render("Admin"),
        )
}
```

### Progress Bar

```go
func ProgressBar(width int, percent float64) string {
    fullWidth := int(float64(width) * percent)
    bar := lipgloss.NewStyle().
        Foreground(lipgloss.Color("62")).
        Width(fullWidth)

    empty := lipgloss.NewStyle().
        Foreground(lipgloss.Color("240")).
        Width(width - fullWidth)

    return bar.Render("█") + empty.Render("░")
}
```

## Best Practices

### Style Definition

```go
// Define styles as package-level variables
var (
    Title = lipgloss.NewStyle().
        Bold(true).
        Foreground(lipgloss.Color("62")).
        FontWeight(600)

    Subtitle = lipgloss.NewStyle().
        Foreground(lipgloss.Color("245")).
        MarginBottom(1)

    Content = lipgloss.NewStyle().
        Padding(1, 2).
        BorderStyle(lipgloss.RoundedBorder())
)
```

### Performance

- Avoid recreating styles in loops
- Cache rendered strings when possible
- Use `lipgloss.ColorSupported()` to skip expensive styling

## Related

- [Glamour](https://charm.land/lipgloss) - Markdown rendering (use glamour skill)
- [Bubble Tea](https://charm.land/bubbletea) - TUI framework (use bubbletea skill)
