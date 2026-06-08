# fleet-midi-pan

_Spatial positioning — put sounds where they belong._

_One of 16 ternary MIDI agents in the [Live Paradigm Fleet](https://github.com/SuperInstance/sailor-workspace)._

---

## Philosophy — Why Ternary?

The Live Paradigm treats musical gestures as ternary operations. Where binary logic
gives yes/no, ternary gives **approve/reject/observe** — a richer cognitive substrate
that maps naturally to music theory, emotional tension, and conversational flow.

This agent implements **ternary decomposition for pan**.

## Architecture

Position in the fleet pipeline:

```
🎤 Voice → OpenSMILE (25 features) → Ghost Track (T-0..T-4 CR predictions)
  → tminus-dispatcher (cue scheduling) → Fleet Conductor (routing)
  → pan (port 2167)
```

## API Reference

| Method | Path | Description |
|--------|------|-------------|
| GET | /health | Health check + agent identity |
| POST | /agent with `{"type":"probe"}` | Liveness probe for fleet-conductor |
| POST | /agent | Process musical data, return ternary analysis |
| POST | / | Direct query with JSON body |

### Response Format

```json
{
  "status": "ok",
  "agent": "fleet-midi-pan",
  "port": 2167,
  "ternary_vector": [0, 0, 0],
  "ternary_invariant": 0,
  "closed_gesture": false
}
```

## Ternary Logic

| Position | +1 | 0 | -1 |
|----------|------|------|------|
| ternary[0] | right | center | left |

## Educational Supplement

Panning places a sound in the stereo field. Left (-1), center (0), right (+1).
It's the most intuitive ternary mapping in the entire fleet — literally three positions.

### The Stereo Spectrum
- **Left (-1)**: L channel, or pan value 0-31 (of 127)
- **Center (0)**: Both channels equal, pan value 48-79
- **Right (+1)**: R channel, or pan value 97-127

### Pan and the Gesture
A sound that moves left-to-right (+1 in ternary[2]) creates a sense of travel.
Combined with dynamics, pan becomes narrative: "the sound walks across the room."

## Fleet Integration

- **Port**: 2167
- **Roles**: spatial
- **Conductor ID**: `pan`
- **Protocol**: HTTP POST to `/2167/agent` with JSON body, 5s timeout
- **Conservation Law**: Σ(Δ_midi) = 4 × Σ(ternary) — closed gestures return to start

## Starting

Local development:

```bash
python3 engine.py --port 2167
```

Or via the fleet start script:

```bash
./scripts/start-fleet-agents.sh
```

## Credits

**Part of the Live Paradigm Fleet** — A ternary cognitive architecture for musical AI.
GitHub: github.com/SuperInstance
Fleet conductor: [sailor-workspace](https://github.com/SuperInstance/sailor-workspace)
