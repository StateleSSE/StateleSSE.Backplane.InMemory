# StateleSSE.Backplane.InMemory

> **⚠️ DEPRECATED**: This package has been consolidated into `StateleSSE.AspNetCore`.
>
> **Migration:** The in-memory backplane is now included in `StateleSSE.AspNetCore`. Simply install that package and use `services.AddInMemorySseBackplane()`.
>
> This package is no longer maintained and will not receive updates.

---

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
