# hearth-voices

Download files for [Hearth](https://hearthreader.com), an offline reading app for Android. The app fetches these on request and verifies each file's SHA-256 before using it. Nothing here is needed to read the code; it is the content the app downloads.

## kokoro-v1.0 — the voice pack (about 110 MB)

| File | Bytes | SHA-256 |
|---|---|---|
| `model.q8.onnx` | 83,513,147 | `961a380650c2963fda98de2c0ff20b1638ff599843b7af55eb2effe49eee2183` |
| `voices.bin` | 28,200,960 | `1c5a5b983d3d50d8586d437a51f3faa2da7919ce76a013c081e65671a3447c29` |
| `tokens.txt` | 687 | `6ebb6bb288f20f3ae8d004d3c2ca27697da27c037d75e81a60e2a6a663f95425` |

The Kokoro-82M text-to-speech model and its voices are by [hexgrad](https://huggingface.co/hexgrad/Kokoro-82M), Apache License 2.0. The ONNX export, voice table and token list come from the sherpa-onnx project's [csukuangfj/kokoro-multi-lang-v1_0](https://huggingface.co/csukuangfj/kokoro-multi-lang-v1_0), Apache License 2.0; `voices.bin` and `tokens.txt` are those files unchanged.

`model.q8.onnx` is that export's `model.onnx` (SHA-256 `b40f62b166ac8164b0627ef48a0b358eda0985e272fb03ef5252e7206305da11`) with every large weight stored as 8-bit integers, one scale per output channel, behind a `DequantizeLinear` node. ONNX Runtime expands the weights on load, so all arithmetic is the same as the original; only the file is smaller (84 MB instead of 326 MB). It is made with `tools/quantize_kokoro.py` from Hearth's source and reproduces bit for bit. The license text is in `kokoro-v1.0/LICENSE.txt`.

## classics-v1 — five public-domain books

Standard Ebooks editions, released by [Standard Ebooks](https://standardebooks.org) under CC0 1.0 (public domain). The texts themselves are public domain worldwide. The files are unchanged copies.

| File | Bytes |
|---|---|
| `robert-louis-stevenson_treasure-island.epub` | 720,634 |
| `jane-austen_pride-and-prejudice.epub` | 831,959 |
| `bram-stoker_dracula.epub` | 633,860 |
| `charles-dickens_a-christmas-carol.epub` | 548,617 |
| `mary-shelley_frankenstein.epub` | 685,797 |

## How the app fetches them

Hearth downloads from this repository's files on the `main` branch, by folder, and falls back to Hugging Face (voice table and token list) or standardebooks.org (books) when this host is unreachable. A download that fails its checksum is deleted and retried.

## Website

The `docs/` folder is the site at https://hearthreader.com (GitHub Pages): a short landing page and the privacy policy at `/privacy`. The policy is generated from `PRIVACY_POLICY.md` in the app's repository.
