# Earcon Workbench

`nvim-speaks` sends earcons as protocol commands with a stable `id`:

```json
{"cmd": "earcon", "id": "completion_accept"}
```

The protocol command does not embed waveform data. The repository keeps the
reference synthesized earcon designs in `earcon-recipes/`, and
`earcon-workbench` renders those recipes to WAV files for auditioning,
comparison, and backend implementation work.

Backends may render an earcon however they need to, but the workbench recipes
are the maintained reference for the intended sound shape.

## Generate WAVs

Generate every recipe and verify the WAV structure:

```bash
./earcon-workbench --all --verify
```

By default, generated files are written under `/tmp/nvim-speaks-earcons`.
Generated WAVs are stereo, 16-bit PCM at 44.1 kHz. They are development outputs
and are not committed.

To play each WAV as it is generated:

```bash
./earcon-workbench --all --play
```

In WSL, playback can fall back to Windows PowerShell when `powershell.exe` is
available.

## Browser Audition Board

Generate an HTML page with audio controls for every selected earcon and variant:

```bash
./earcon-workbench --html
```

Open `/tmp/nvim-speaks-earcons/index.html` in a browser with audio output to
compare the generated WAV files.

## Focused Preview

Generate or preview one cue:

```bash
./earcon-workbench --id completion_accept --variant bright --play --repeat 3
```

List available recipe ids and variants:

```bash
./earcon-workbench --list
```

Current variants are `default`, `soft`, `bright`, and `compact`.

## Recipe Files

Each earcon recipe is one JSON file in `earcon-recipes/`. A recipe has
an `id`, optional metadata such as `description`, and a `voices` array. Voices
support:

- `waveform`
- `duration_ms`
- `delay_ms`
- `gain`
- `pan`
- `pitch`
- `adsr`
- optional simple filters

Shared variant transforms live in `earcon-recipes/variants.json`.

Some recipe ids may be experimental candidates. The protocol reference in
`../nvim-speaks.nvim/docs/protocol.md` lists the ids currently emitted by
plugin speech rendering and advertised by maintained backends.
