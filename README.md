# StateleSSE.Backplane.InMemory

In-memory backplane for single-server SSE deployments.

## Installation

```bash
dotnet add package StateleSSE.Backplane.InMemory
```

## Setup

```csharp
using StateleSSE.Backplane.InMemory.Infrastructure;

builder.Services.AddSingleton<ISseBackplane, InMemoryBackplane>();
```

## Usage

```csharp
using StateleSSE.AspNetCore;

[HttpGet("events")]
public async Task StreamEvents([FromQuery] string gameId)
{
    var channel = ChannelNamingExtensions.Channel<PlayerJoinedEvent>("game", gameId);
    await HttpContext.StreamSseAsync<PlayerJoinedEvent>(backplane, channel);
}

public async Task PublishEvent(string gameId)
{
    await backplane.PublishToGroup($"game:{gameId}:PlayerJoinedEvent", evt);
}
```

## When to Use

- Development and testing
- Single-server deployments
- Scenarios without horizontal scaling

For production horizontal scaling, use `StateleSSE.Backplane.Redis`.

## License

MIT
