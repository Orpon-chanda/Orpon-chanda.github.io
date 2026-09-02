# Deploy to GitHub Pages — Orpon Portfolio

## One-click local preview
```bash
python3 -m http.server 8000 --directory /home/touhid/opendesign
# open http://localhost:8000/mockups/orpon-portfolio/
```

## Publish to `Orpon-chanda.github.io` (or any repo's gh-pages)

### Option A — User site (https://orpon-chanda.github.io)
```bash
gh repo create Orpon-chanda/Orpon-chanda.github.io --public --clone
cd Orpon-chanda.github.io
cp -r /home/touhid/opendesign/mockups/orpon-portfolio/* .
# photo.jpg + index.html are all you need
git add .
git commit -m "feat: lab-ledger portfolio for Orpon Chanda — 8 repos, responsive"
git push -u origin main
# Settings > Pages > Source: Deploy from branch (main / root) -> live in ~60s
```

### Option B — Project site in existing repo
```bash
cd /path/to/Orpon-chanda/PID_LFR_LU
mkdir -p docs
cp /home/touhid/opendesign/mockups/orpon-portfolio/index.html docs/
cp /home/touhid/opendesign/mockups/orpon-portfolio/photo.jpg docs/
git add docs && git commit -m "docs: portfolio" && git push
# Settings > Pages > Source: main / docs
```

## READMEs — push to each repo
```bash
# Example for PID_LFR_LU:
gh repo clone Orpon-chanda/PID_LFR_LU
cp /home/touhid/opendesign/readmes/orpon-chanda/README-PID_LFR_LU.md PID_LFR_LU/README.md
cd PID_LFR_LU && git add README.md && git commit -m "docs: detailed README — wiring, PID, usage" && git push

# Repeat for all 8:
# Bluthoot_Car       -> README-Bluthoot_Car.md
# PWM                -> README-PWM.md
# MindSpare1         -> README-MindSpare1.md
# Arduino_Currency_Counter_using_IR_and_Color_Sensor -> README-Arduino_Currency_Counter.md
# RFID_4             -> README-RFID_4.md
# github             -> README-github-LFR_VAI.md (rename repo suggestion: LFR_VAI_CODE)
# Orpon-chanda       -> README-profile.md  (commit to main -> shows on profile)
# PID_LFR_LU         -> README-PID_LFR_LU.md
```

## Revoke the leaked PAT
Settings > Developer settings > Personal access tokens > Tokens (classic) > Delete `github_pat_11ARJ...`
Then create a fine-grained token (read-only, expiry 7 days) if you need pushes from CLI — or use `gh auth login`.

## Email fix
Current email `934906Rj` is not a valid address. Get full address (e.g. `orpon@lus.ac.bd`) then:
```
# in index.html search and replace 934906Rj -> real email (2 places: hero + contact)
```
