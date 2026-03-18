---
name: fantasy
description: "Fantasy (charm.land/fantasy) LLM provider abstraction for Go. Use when working with LLM providers, creating AI agents, handling chat completions, or managing provider-specific features. Triggers on fantasy, llm provider, chat completion, ai agent, LanguageModel, ProviderOptions."
---

# Fantasy - LLM Provider Abstraction

> Unified interface for multiple LLM providers (OpenAI, Anthropic, Gemini, etc.)

## Overview

Fantasy provides a unified API for interacting with various LLM providers. It abstracts away provider-specific differences and offers a consistent interface for building AI applications.

## Installation

```bash
go get charm.land/fantasy
```

## Core Concepts

### LanguageModel

The main interface for interacting with LLMs:

```go
import "charm.land/fantasy"

model, err := fantasy.NewLanguageModel(ctx, fantasy.LanguageModelOptions{
    Provider: "openai",
    Model:    "gpt-4o",
    APIKey:   os.Getenv("OPENAI_API_KEY"),
})
```

### Agent

High-level agent for tool-augmented conversations:

```go
agent := fantasy.NewAgent(
    fantasy.WithSystemPrompt("You are a helpful assistant."),
    fantasy.WithTools(tool1, tool2),
)

result, err := agent.Run(ctx, fantasy.AgentStreamCall{
    Messages: []fantasy.Message{
        fantasy.NewUserMessage("Hello!"),
    },
})
```

## Provider Configuration

### OpenAI

```go
model, err := fantasy.NewLanguageModel(ctx, fantasy.LanguageModelOptions{
    Provider: "openai",
    Model:    "gpt-4o",
    APIKey:   apiKey,
})
```

### Anthropic

```go
model, err := fantasy.NewLanguageModel(ctx, fantasy.LanguageModelOptions{
    Provider: "anthropic",
    Model:    "claude-sonnet-4-20250514",
    APIKey:   apiKey,
})
```

### Google Gemini

```go
model, err := fantasy.NewLanguageModel(ctx, fantasy.LanguageModelOptions{
    Provider: "gemini",
    Model:    "gemini-2.0-flash",
    APIKey:   apiKey,
})
```

## Streaming Responses

```go
agent := fantasy.NewAgent(/* ... */)

stream, err := agent.Stream(ctx, fantasy.AgentStreamCall{
    Messages: messages,
})

for {
    event, err := stream.Recv()
    if errors.Is(err, io.EOF) {
        break
    }
    if err != nil {
        return err
    }
    
    switch e := event.Content.(type) {
    case fantasy.ContentText:
        fmt.Print(e.Text)
    case fantasy.ContentToolCall:
        fmt.Printf("Calling tool: %s\n", e.Name)
    }
}
```

## Tool Integration

### Defining Tools

```go
calculator := fantasy.AgentTool{
    Name:        "calculator",
    Description: "Perform mathematical calculations",
    InputSchema: fantasy.InputSchema{
        Type: "object",
        Properties: map[string]fantasy.Property{
            "expression": {
                Type:        "string",
                Description: "Mathematical expression to evaluate",
            },
        },
        Required: []string{"expression"},
    },
    Handler: func(ctx context.Context, input map[string]any) (any, error) {
        expr := input["expression"].(string)
        // Parse and evaluate
        result, err := eval(expr)
        return map[string]any{"result": result}, err
    },
}
```

### Tool Callbacks

```go
result, err := agent.Stream(ctx, fantasy.AgentStreamCall{
    OnToolCall: func(tc fantasy.ToolCallContent) error {
        fmt.Printf("Tool called: %s\n", tc.Name)
        return nil
    },
    OnToolResult: func(result fantasy.ToolResultContent) error {
        fmt.Printf("Tool result: %v\n", result.Output)
        return nil
    },
})
```

## Message Types

```go
// User message
fantasy.NewUserMessage("Hello!")

// System message
fantasy.NewSystemMessage("You are a helpful assistant.")

// Assistant message
fantasy.NewAssistantMessage("Hello! How can I help?")

// Tool result message
fantasy.NewToolResultMessage(toolCallID, "result here")
```

## Error Handling

```go
var fantasyErr *fantasy.Error
var providerErr *fantasy.ProviderError

result, err := agent.Run(ctx, call)
if err != nil {
    if errors.As(err, &providerErr) {
        fmt.Printf("Provider error: %s - %s\n", 
            providerErr.Code, providerErr.Message)
    }
    return err
}
```

## Provider Options

Customize provider behavior:

```go
options := fantasy.ProviderOptions{
    MaxTokens:   4096,
    Temperature: 0.7,
    TopP:        0.9,
}

result, err := agent.Run(ctx, fantasy.AgentStreamCall{
    ProviderOptions: options,
})
```

## Common Patterns

### Retry Logic

```go
result, err := agent.Stream(ctx, fantasy.AgentStreamCall{
    OnRetry: func(err *fantasy.ProviderError, delay time.Duration) {
        fmt.Printf("Retrying in %v due to: %s\n", delay, err.Message)
    },
})
```

### Handling Reasoning

```go
result, err := agent.Stream(ctx, fantasy.AgentStreamCall{
    OnReasoningStart: func(id string, content fantasy.ReasoningContent) error {
        fmt.Println("Reasoning started...")
        return nil
    },
    OnReasoningEnd: func(id string, content fantasy.ReadingContent) error {
        fmt.Println("Reasoning completed.")
        return nil
    },
})
```

### Stop Conditions

```go
result, err := agent.Stream(ctx, fantasy.AgentStreamCall{
    StopWhen: []fantasy.StopCondition{
        func(steps []fantasy.StepResult) bool {
            return len(steps) >= 10 // Max 10 steps
        },
        func(steps []fantasy.StepResult) bool {
            last := steps[len(steps)-1]
            return last.FinishReason == fantasy.FinishReasonStop
        },
    },
})
```

## Related Skills

- [crush-config-helper](https://skills.sh/crush-config-helper) - Configure LLM providers in Crush
- [golang-pro](https://skills.sh/golang-pro) - Go best practices
