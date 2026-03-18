---
name: catwalk
description: "Catwalk (charm.land/catwalk) provider sync and embedded providers for Go. Use when configuring LLM providers, syncing provider lists with caching, using embedded provider defaults, or handling provider fallback logic. Triggers on catwalk, provider sync, embedded providers, provider cache, GetProviders, ErrNotModified."
---

# Catwalk - Provider Sync & Embedded Providers

> Charm's provider synchronization library with caching and embedded fallback

## Overview

Catwalk provides:
- Provider list synchronization from remote sources
- Local caching with ETag support
- Embedded provider defaults for offline/fallback use
- Thread-safe provider management

## Installation

```bash
go get charm.land/catwalk
```

## Core Concepts

### Provider Model

```go
import "charm.land/catwalk/pkg/catwalk"

type Provider struct {
    ID          string   // Unique provider ID
    Name        string   // Display name
    Description string   // Provider description
    Models      []Model  // Available models
    AuthType    string   // Authentication type
    Website     string   // Provider website
}
```

### Client Interface

```go
type catwalkClient interface {
    GetProviders(ctx context.Context, etag string) ([]catwalk.Provider, error)
}

var ErrNotModified = errors.New("providers not modified")
```

## Usage Patterns

### Basic Sync with Fallback

```go
import (
    "charm.land/catwalk/pkg/catwalk"
    "charm.land/catwalk/pkg/embedded"
)

type providerSyncer struct {
    sync     sync.Once
    result   []catwalk.Provider
    cache    cache
    client   catwalkClient
}

func (s *providerSyncer) Get(ctx context.Context) ([]catwalk.Provider, error) {
    s.sync.Do(func() {
        // Try cache first
        cached, etag, _ := s.cache.Get()
        
        // Fall back to embedded if cache empty
        if len(cached) == 0 {
            cached = embedded.GetAll()
        }
        
        // Fetch fresh providers
        result, err := s.client.GetProviders(ctx, etag)
        if errors.Is(err, catwalk.ErrNotModified) {
            s.result = cached
            return
        }
        if err != nil {
            s.result = cached  // Fall back on error
            return
        }
        
        s.result = result
        s.cache.Store(result)
    })
    return s.result, nil
}
```

### With Auto-Update Toggle

```go
func (s *providerSyncer) Init(client catwalkClient, path string, autoupdate bool) {
    s.client = client
    s.cache = newCache(path)
    s.autoupdate = autoupdate
}

func (s *providerSyncer) Get(ctx context.Context) ([]catwalk.Provider, error) {
    if !s.autoupdate {
        return embedded.GetAll(), nil
    }
    // ... sync logic
}
```

## Embedded Providers

Access built-in provider list:

```go
providers := embedded.GetAll()
for _, p := range providers {
    fmt.Printf("Provider: %s (%s)\n", p.Name, p.ID)
}
```

## Error Handling

```go
result, err := client.GetProviders(ctx, etag)

switch {
case errors.Is(err, catwalk.ErrNotModified):
    // Use cached providers
    return cached, nil
    
case errors.Is(err, context.DeadlineExceeded):
    // Timeout - use cached/fallback
    return cached, nil
    
case err != nil:
    // Other errors - fallback to cache
    return cached, nil
    
case len(result) == 0:
    // Empty result - must have at least cached
    return cached, errors.New("empty providers list")
}
```

## Testing with Mock Client

```go
type mockCatwalkClient struct {
    providers []catwalk.Provider
    err      error
    callCount int
}

func (m *mockCatwalkClient) GetProviders(ctx context.Context, etag string) ([]catwalk.Provider, error) {
    m.callCount++
    return m.providers, m.err
}

// In tests:
client := &mockCatwalkClient{
    providers: []catwalk.Provider{{Name: "Test", ID: "test"}},
}
syncer := &catwalkSync{}
syncer.Init(client, t.TempDir()+"/providers.json", true)
```

## Key Files in Crush

- `internal/config/catwalk.go` - Provider sync implementation
- `internal/config/catwalk_test.go` - Sync tests with mock client

## Related Skills

- [fantasy](https://skills.sh/fantasy) - LLM provider abstraction
- [crush-config-helper](https://skills.sh/crush-config-helper) - Provider configuration
