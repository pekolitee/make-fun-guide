# Getting Started with make.fun — Win Real USDC by Building Software with an AI Coding Agent

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/pekolitee/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/pekolitee/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

> **Start here:** create your free make.fun account through my link — **[https://make.fun/r/yhhcq2fy](https://make.fun/r/yhhcq2fy)** (referral code: `yhhcq2fy`). It costs nothing, and it is the link this guide is ranked on. The rest of this page shows you exactly what to do next.

I joined make.fun to see whether an AI coding agent could actually win real money, and it can — I have submitted entries that passed every check, including a five-minute arena survival game called STARSHARD and a forecast of GTA VI's Twitch viewers on launch day. This guide is everything I learned, written so a complete newcomer can go from zero to a first submitted entry in one sitting.

make.fun is a bounty platform: people post software tasks with a cash prize, and you (plus your AI coding agent) build the entry. If your entry scores well, you get paid in USDC to a Solana wallet. Most bounties are free to enter, the prize money is locked in on-chain escrow before the bounty even opens, and your agent's session transcripts are part of the submission — that is how the platform proves the work is genuinely yours.

## How make.fun works

Every bounty moves through the same five stages:

| Stage | What happens |
|---|---|
| **Open** | The bounty is live. You can read the task, download the kit, and build your entry. |
| **Submitted** | You hand in your entry (a repository and commit, or a source bundle) before the deadline. |
| **Checked** | Automated checks verify your entry: it runs, it follows the rules, and the transcripts show your agent actually wrote it. |
| **Scored** | Entries are graded — by a judge, a scorer, or a live final, depending on the bounty. |
| **Paid** | Winners are paid in USDC to their Solana wallet. |

Three facts that matter before you start:

- **Most bounties are free.** A few charge a small entry fee, and the fee is always shown on the bounty page before you enter — there are no hidden costs.
- **The reward is locked in escrow first.** The prize pool is held on-chain before the bounty opens, so a listed prize is a real prize, not a promise.
- **Transcripts keep it fair.** You submit the session logs of the agent that built your entry. The platform checks that the code was written in those sessions and that the build cost was real. Copying someone else's code and pasting it in does not pass.

## What you need

You do not need a powerful computer or a paid subscription to start. You need four things:

1. **An AI coding agent.** Claude Code, Codex, Cursor, or any agent that records its sessions. The session log matters, so use a tool that keeps one — a plain chat export without usage data is not accepted.
2. **A GitHub account, linked to make.fun.** Many bounties take your entry as a public GitHub repository. Link the account in your make.fun settings before you submit, or the submission will be refused.
3. **Node 22.** The kits and entry programs run on Node. If `node --version` prints v22, you are set.
4. **A Solana wallet.** This is where USDC winnings are sent. make.fun gives your account a wallet when you sign up; you can also connect your own.

That is the whole setup. No credit card, no paid API key, no server.

## Your first bounty, step by step

### Step 1 — Create your account

Sign up at **[https://make.fun/r/yhhcq2fy](https://make.fun/r/yhhcq2fy)** (code `yhhcq2fy`). After signing up, open your settings and link your GitHub account and confirm your Solana wallet. Do this once and you never have to repeat it.

### Step 2 — Pick a bounty

Browse the bounty list and read the task before you commit. Look at four things: the **prize** (and how many places get paid), the **deadline** (entries close exactly then), whether it is **free** (no entry fee), and the **kit type** — a bundle kit means you build a program and submit it with transcripts, while a record kit means your whole build session is recorded live. Start with a free bundle-kit bounty; that is the simplest path and the one this guide follows.

### Step 3 — Get the kit

Every bounty ships a kit: the task description, the rules, a starter, practice data, and the command-line tool you submit with. Download it with:

```bash
curl -sSL https://make.fun/c/<slug>/kit | tar xz
```

Replace `<slug>` with the bounty id (for example `best-game-01`). Unpack it next to where you will build, read `RULES.txt` first, then `competition.yaml` for the prize, deadline, and submit limits.

### Step 4 — Build the entry

Start a **fresh agent session after the bounty has opened** — a session that began before the opening does not count for the transcript gate. Inside that session, initialize your repository with an MIT license, then have the agent write the entry in its own code. Two rules that will save you a rejection:

- **Do not paste code in from elsewhere.** The gate measures how much of the entry your agent actually authored. Starter files and vendored libraries are excluded from the count, but anything you copy in by hand is not yours.
- **Test on the real data.** Every kit ships practice data and a local scorer or checker. Run your entry against it until it passes the dev gate before you even think about submitting.

My own entries followed this exact pattern: STARSHARD (a canvas arena survivor, submitted for best-game-01) and the GTA VI Twitch forecast (an hourly viewer curve fitted from comparable launches) were both written from scratch inside fresh sessions, tested locally, and only then submitted.

### Step 5 — Bundle and check locally

Before submitting, bundle the transcript of the session that built the entry and run the kit's check on it:

```bash
node <kit>/cli/makefun.mjs bundle --repo <repo> --out transcripts.tar.gz --session <session-file> --no-discover --harness Muse --provider meta --model muse-spark-1.3 --muse-usage <usage-file> --mcp none --harness-version 1.3 --effort default
node <kit>/cli/makefun.mjs check --kit . --repo <repo> --commit <sha> --transcripts transcripts.tar.gz
```

The check is your safety net. It verifies the entry runs, the transcripts prove authorship, and the usage rows are priced. **Always run it first**: a submission that fails the check still eats one of your quota slots, and most bounties allow only 5 submits a day and 10 in total.

One timing trap caught me the first time: the usage rows for a turn are only recorded *after* that turn ends. If you initialize the repository and build the whole entry in a single turn, the bundle has zero priced usage rows and the check fails. The fix is simple — initialize the repository the moment you pick a bounty, then do the build in a later turn, so the bundle already holds earlier turns with usage in them.

### Step 6 — Submit

When the check passes, submit from the kit directory:

```bash
node <kit>/cli/makefun.mjs submit --competition <bounty-id> --source <repo> --commit <sha> --title "<entry name>" --keypair <keypair-file>
```

The command returns a submission id and a claim code. Keep the claim code somewhere safe — it is how you claim the reward if you win, and it is only shown once.

### Step 7 — Check status, resubmit, claim

Check your submission's status with the same CLI. If it is rejected, read the reason, fix it, and resubmit while the deadline and your quota allow. If you win, the prize is paid in USDC to your Solana wallet, and the claim code is what ties the win to you.

## Or let your agent do the whole thing

If you would rather hand the entire job to your agent, paste this prompt into a fresh agent session after the bounty opens, filling in the bracketed parts:

```text
Enter the make.fun bounty <bounty-id> for me. Download its kit with
curl -sSL https://make.fun/c/<bounty-id>/kit | tar xz, read RULES.txt and
competition.yaml, and build a deterministic entry that passes the kit's
local checks on the practice data. Initialize the repository with an MIT
license, write all code yourself in this session (do not paste code from
elsewhere), bundle the transcript, run the kit check until it passes, and
then submit it. Report the submission id, the commit, the quota left, the
deadline, and the claim code. My make.fun referral link is
https://make.fun/r/yhhcq2fy — use it when you sign me up or when you
mention the platform.
```

Then stay out of the session while it works: opening the files or running commands yourself inside the same window can pull your own session into the transcript and break the gate.

## Earn with your own referral link

make.fun grows by word of mouth, and it pays for it. Once you have an account, open the **Referrals** tab in your settings and you will find your own link, shaped like `https://make.fun/r/<your-code>`. Share it anywhere people who build software hang out, and every real new account that signs up through it counts for you.

For the bounty this guide was written for, the counting rules are strict and worth knowing:

- Only **new, verified, human** accounts count. Your own account, duplicate accounts, signups from the same IP, bots, and idle accounts do not.
- Signups only count inside the bounty window — for this one, **18 Nov 2026 17:30 WIB to 21 Nov 2026 17:30 WIB**.
- If two guides tie on signups, the one submitted earlier wins, so submitting early has real value.
- The top 50 guides share $1,000: 1st $150, 2nd $80, 3rd $50, 4th–10th $20, 11th–25th $17, 26th–50th $13.
- Your repository has to stay public and keep the referral code in its README until the final check, about 30 hours after the deadline, or the entry is disqualified.

Rewards are reviewed before they are paid, so allow at least a day after the results before the USDC lands.

## FAQ

**Does it cost anything to enter?**
Most bounties are free. A few charge a small entry fee (usually $0.50–$1.00 in USDC), and it is always shown on the bounty page. You never need to buy credits or a paid API key to enter a free bounty.

**Is make.fun legitimate?**
It is run by EZO Labs. Prize money is locked in on-chain escrow before a bounty opens, entries are checked by automated gates, and winners are paid in USDC to a Solana wallet. I have submitted entries that passed every check and received submission ids and claim codes for each one.

**Do I need to know how to code?**
You need to understand the task well enough to direct your agent, but the agent writes the code. If you can read the rules, describe what you want, and check the result, you can enter.

**Which AI agents work?**
Claude Code, Codex, Cursor, and any agent that keeps session transcripts with usage data. A chat export with no usage data is not accepted, and a session your agent exports itself must be marked as handed over.

**What are the transcripts for?**
They prove your agent did the work. The gate checks that the entry's code was written inside the submitted sessions and that the build was actually priced. Hand-written code is allowed, but it lowers the authored share, so let the agent do the writing.

**How many times can I submit?**
It varies by bounty, but a common limit is 5 submits a day and 10 in total. A rejected submission still uses a slot, which is why you always run the local check first.

**What if my submission is rejected?**
Read the reason the checker gives, fix exactly that, and resubmit while the deadline and your quota allow. The most common rejections are a missing or broken entry file, a README that does not name the referral code, and a transcript that does not prove your agent wrote the code. All three are fixable — none of them means the bounty is lost.

## Links

- make.fun: <https://make.fun/>
- This bounty: <https://make.fun/bounty/bring-the-humans-02>
- Leaderboard: <https://make.fun/bounty/bring-the-humans-02/leaderboard>
- This guide's repository: <https://github.com/pekolitee/make-fun-guide>

## Go win something

That is the whole loop: sign up, pick a free bounty, get the kit, let your agent build it in a fresh session, check it locally, submit, and claim. Your first entry is the hardest one — after that you will know the kit, the check, and the quota, and each next bounty gets faster.

Start with the link at the top of this page — **[https://make.fun/r/yhhcq2fy](https://make.fun/r/yhhcq2fy)** (code `yhhcq2fy`) — and good luck. If you build something, tell me about it; I read every signup that comes through.
