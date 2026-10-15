# DarkCircle `unshield` ceremony — contributor attestation

**How this file is used**

- **Coordinator:** copy this template once per contributor, fill in *Section A*, and send it
  together with the current chain-head zkey and its sha256.
- **Contributor:** complete *Section B*, then publish this exact file as a **signed git
  commit** in any public GitHub repo you own — GitHub must show the green "Verified" badge.
  Send the coordinator the commit URL. Setup: see "Signing with GitHub" below.
- **Coordinator:** run the *Section C* checklist before adopting the contribution
  (`{"action":"adopt","file":"unshield_XXXX.zkey","name":"<handle>"}` in the conductor).

---

## Ceremony constants — your attestation binds to exactly these

| item | value |
|---|---|
| circuit | `circuits/unshield.circom` (frozen; commit hash published by coordinator) |
| r1cs sha256 | `1687738ec20ee5298c6f0fefc8ea8ceef8c528b0be9f592cfe0ea6ea4ec34779` |
| r1cs shape | 25,921 constraints · 10 public inputs |
| powers of tau | `powersOfTau28_hez_final_15.ptau` (perpetual ceremony, 54 contributions + beacon) |
| ptau sha256 | `3ef2ecc5b75d687048cf2d59195119b42fb07c5af639c5f283d84bfa69829e7f` |
| ptau blake2b (official) | `982372c867d229c236091f767e703253249a9b432c1710b4f326306bfa2428a17b06240359606cfe4d580b10a5a1f63fbed499527069c18ae17060472969ae6e` |
| beacon pre-announcement | Bitcoin mainnet block **971508** |
| pre-announcement commitment | `49236182641a9151bed2c797efb76fe503394c8c80755f5de3188ab6a06cd57d` |

If any of these values differ from what you independently observe (e.g. the ptau hash
published in the snarkjs README), **stop and contact the coordinator — do not contribute.**

---

## Signing with GitHub (contributor how-to)

One-time setup — reuses the SSH key you already push with:

```
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519.pub
git config --global commit.gpgsign true
```

Then add that same public key on GitHub as a **Signing Key**:
github.com/settings/keys → "New SSH key" → Key type: **Signing Key**.
(A GPG key uploaded to GitHub works equally well if you already have one.)

Per attestation:

```
mkdir darkcircle-attestation && cd darkcircle-attestation && git init
copy the filled slot-XX-attestation.md into this folder
git add slot-XX-attestation.md
git commit -S -m "attestation: contribution #N by <handle>"
git remote add origin git@github.com:<you>/darkcircle-attestation.git
git push -u origin main
```

Send the coordinator the commit URL
(`https://github.com/<you>/darkcircle-attestation/commit/<sha>`). It must show
"Verified" on GitHub. The commit SHA binds this file's exact content — edit after
signing and the URL no longer matches.

---

## Section A — filled by the coordinator

- Contribution index: **`5`** (must be previous index + 1; the chain is strictly sequential)
- Input file: `unshield_0004.zkey` — sha256: `17633c83a6513b06fa05d01a64faa807645fac15f8c56a707cf412ef5fe36ca3`
- Output file you must produce: `unshield_0005.zkey`
- Contributor handle: `cyberWarlock`
- Coordination channel / fetch URL for the input file: https://gofile.io/d/d99DzBdE (verify sha256 after download)

## Section B — filled by the contributor

- Date/time (UTC): `8:50:42 PM`
- Machine / environment (OS, online or air-gapped): `windows`
- [x] I verified the **input zkey sha256** matches Section A exactly.
- [x] I verified the **r1cs sha256** and **ptau hashes** match the ceremony constants above.
- [x] (Recommended) I ran `npx snarkjs zkey verify unshield.r1cs powersOfTau28_hez_final_15.ptau <input.zkey>`
      on the input file and saw `ZKey Ok!` before contributing.
- Exact command run: `npx snarkjs zkey contribute unshield_0004.zkey unshield_0005.zkey --name="cyberWarlock"`

- Output zkey sha256: `5cf48cde6288c237f66b8198f13d6471fbd438f94b0c403973bae8acf29aa83f`  *(pre-filled by coordinator from the verification report — confirm it matches your own computation)*
- Contribution hash printed by snarkjs: `37f9486c93ca532cffb89d6e901c70dfec70b73145868bceb7bb7258285ea193248038cd327f9652d22c59bce571a5c9780653aa59447ff9c2c84f2cc06a78d5`  *(pre-filled from the verification report — confirm it matches what your terminal printed)*

**Entropy statement.** I generated my contribution entropy by
`A short poem and random mashing` (e.g. keyboard mashing plus OS RNG, dice, a hardware RNG).
I have **destroyed all copies** of the entropy and every intermediate secret, and I retained
nothing that could reconstruct my contribution's toxic waste.

**Independence statement.** To the best of my knowledge I am independent of the coordinator
and of the other contributors, and I have not colluded to reconstruct anyone else's secrets.

**Signature.** This exact file is published as a signed commit on GitHub:

- GitHub handle: `cyberWarlock`
- Commit URL (must show "Verified"): `https://github.com/TheCyberWarlock/darkcircle-attestation/commit/2b2d6c1cf442d990291f06439571b135a542a72b`
- Commit SHA: `2b2d6c1cf442d990291f06439571b135a542a72b`

## Section C — coordinator verification checklist

- [ ] Output zkey sha256 matches Section B (recompute locally).
- [ ] Contribution hash matches what `snarkjs zkey verify` reports for the new head.
- [ ] Commit verifies on GitHub — fetch
      `https://api.github.com/repos/<owner>/<repo>/commits/<sha>` and confirm
      `"verification": { "verified": true, "reason": "valid" }`.
- [ ] Commit author login matches the claimed GitHub handle, and the committed file's
      content is identical to the returned attestation (the SHA binds the content).
- [ ] Index is previous + 1; no chain gap; input sha256 equals our published head.
- [ ] `npx snarkjs zkey verify` passes on the new head file.
- [ ] Adopted via conductor `adopt` action; transcript updated; file passed to next contributor.

*Any mismatch: stop the chain, do not adopt, investigate from the last good file (guide §4).*
