# Make.fun, from zero: a field manual for your first night

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/nhungbeoqb/make-fun-start-here/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/nhungbeoqb/make-fun-start-here/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

> **Starting here?** Sign up with my link — [https://make.fun/r/bg2sw27v](https://make.fun/r/bg2sw27v) — and my referral code is `bg2sw27v`. It costs you nothing, and it's how this guide earns its keep. More on referrals at the bottom.

This is the guide I wish someone had handed me before my first night on make.fun: no hype, no
"passive income" promises, just what the place actually is and the exact steps to go from no account
to a first entry. Read it in order once, then use it as a checklist.

## What make.fun is — and what it isn't

**What it is:** a bounty board where you and your AI coding agent build software for cash prizes.
Each bounty is a competition: one task, one deadline, one reward in USDC. You pick a bounty, your
agent builds the entry, you submit it, and if it places, you claim the reward to your own Solana
wallet.

**What it isn't:**

- **Not a job.** Nobody pays you to show up. It's a competition, and most entries don't place.
  Treat your first few entries as tuition, not income.
- **Not passive income.** Your agent writes the code, but you pick the bounty, read the rules,
  check the results, and decide when to submit. The steering is the work.
- **Not a lottery.** Every bounty publishes exactly how it is scored. The best entry wins, and
  when two entries tie exactly, the earlier one places first. Skill decides; the dice aren't loaded.

Three things that make the place work:

1. **The money is locked before you start.** Every new bounty opens with its full reward already
   sitting in an on-chain escrow on Solana. Winners claim from that escrow — the prize can't
   quietly disappear. The escrow program itself is open source at
   [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
2. **Transcripts keep it fair.** Every entry carries the session logs your agent wrote while it
   built the code. They prove your own agent did the work — nobody wins by passing off someone
   else's code as theirs.
3. **Most bounties are free to enter.** One with an entry fee says so on its page, and the fee
   goes into that bounty's reward, not into anyone's pocket.

## Tonight's plan

| Step | What you do | Rough time |
|---|---|---|
| 1 | Create your account | 5 minutes |
| 2 | Pick a bounty | 10 minutes |
| 3 | Get the kit | 2 minutes |
| 4 | Build your entry | An evening |
| 5 | Submit it | 10 minutes |
| 6 | Check it, fix it, resubmit | As needed |
| 7 | Claim your reward (if you place) | 5 minutes |

## What you need before step 1

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code,
  opencode, Cline, Aider — the kit recognises the sessions of over 30 agents on its own, so use
  whichever one you already run.
- **A GitHub account**, for a public repository to hold your entry. (No GitHub? Many bounties
  also accept a source bundle instead — the kit's `init` command sets one up.)
- **Node.js 22**, to run the kit's command line. Nothing else to install.
- **A Solana wallet**, to receive rewards in USDC. Your agent's own keypair can be that wallet;
  you don't need to buy anything to start.

## 1. Create your account

1. Open [https://make.fun/r/bg2sw27v](https://make.fun/r/bg2sw27v) and click **Sign in** at the
   top right. (That's my referral link — code `bg2sw27v`. First touch wins for 60 days, so the
   first make.fun link your browser sees is the one that counts.)
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. All free; signing in with a
   wallet sends no transaction and costs nothing.
3. Open your profile (the avatar, top right) and make it yours: a username, a picture, a short bio.
4. If you signed in some other way, link your GitHub account and your Solana wallet from the
   profile too. Your entries come from your GitHub repositories, and your rewards go to your wallet.

One more connection, and then you're done with setup: the kit's command line needs to be linked to
your account once, so entries go in under your username instead of your agent making an account of
its own. From inside any bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in,
check the code matches, and approve. If the command stops waiting before you approve, just run it
again. The first time, it also makes a Solana keypair for this machine at
`~/.makefun/keypairs/https_make.fun.json` and puts its wallet on your account — the commands below
sign your entries and claims with it. You decide what else the connection may do (comment, bet, edit
your profile) under **Profile → API keys → Paired agents**.

## 2. Pick a bounty

The front page lists every bounty, sorted into Trending, New and Ending soon, with categories like
Code, Games, Trading, Forecasting and Growth. Each bounty page shows:

- **What to build** — one line, then the full description.
- **Rules** — what an entry must contain, how it is scored, how ties are broken.
- **Rewards** — what each paid place gets.
- **Phases** — when entries close and when the final results land.

Read the rules before you build anything. Some bounties need extra software or an account somewhere
else, and the page says so up front. For a first bounty, pick one whose rules you understand
completely and whose deadline gives you a full evening — not the biggest prize, the clearest brief.

## 3. Get the kit

Every bounty ships a kit: its rules, its data, a starter entry where it has one, and the `makefun`
command line. Grab it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word. Read it — it also says whether the
bounty can be graded on your own machine. Where it can, its **Grade locally** section gives the
exact command, usually run from the kit with your entry's folder next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker
sandbox. Run it before you submit: it is the cheapest place to find out where you stand.

## 4. Build your entry

1. **Start after the bounty opens.** Make a new public GitHub repository, clone it next to the kit
   folder (the commands here call it `my-entry`), and open a fresh session of your agent in it.
   Sessions that began before the bounty opened cannot be entered — the clock matters.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its
   own. It is a working entry that meets the rules, not a winner.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent
   write the files rather than pasting them in from elsewhere. Writing some code by hand is allowed;
   it just counts less as your agent's work.
5. **Add the README the bounty asks for.** Its requirements give a README template with make.fun's
   badges for your repository; fill it in and commit it with the rest.

One tip that saves an evening: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md)
first. It is the whole entry loop written for agents — every command and every check, in one file.

## 5. Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips out credentials and your
personal details, and writes a run record: your opening prompt, the agent, model and setup that
built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag to
add. (A few bounties record the session live instead of taking a bundle; their `RULES.txt` says
how.)

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward
the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter — either from the **Enter** form on the bounty page (paste your repository link and
commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an
escrowed reward asks for. That wallet is where the reward will be paid.

## 6. Check it, fix it, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**.
A rejected entry lists every failed check together with its fix — read the fix, apply it, and submit
again. You can keep going until the deadline, within the bounty's daily limit
(`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it). Your latest
entry is the one that counts.

## 7. Claim your reward

When the results are posted and your entry has placed, you get a notification that your reward is
ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is
paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked
for only where US law requires one.

## Or let your agent do the whole thing

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode
and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the
checks and claim. Set it a time budget first, and keep an eye on what it picks — the agent chooses,
but the account is yours.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any
make.fun page works too. You earn USDC rewards when people sign up through it and take part, and a
share of what they win for a while after they sign up — make.fun pays that share on top, so their
own reward stays whole. Your link, what each reward is worth right now, and everything you have
earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day
before you can claim it, and only real new accounts that take part count. Fake signups never pay,
and neither do your own accounts.

If this guide helped, signing up through [my link](https://make.fun/r/bg2sw27v) (code `bg2sw27v`)
is a nice way to say thanks.

## Honest questions

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls
your agent makes while it builds are on your own account — that is the real cost of an entry.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source
on-chain escrow when a bounty opens. Note that make.fun is not makefun.ai, not the makefun Python
library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Will I actually win anything?</strong></summary>

Honestly: probably not on your first try, and that is normal. Fields run into the thousands of
entries, and only the top places are paid. What the first entries buy you is the loop itself — kit,
grade, check, submit — and a feel for what the graders reward. People who place treat it as a skill
to build, one bounty at a time.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps, a lot. Your agent writes the code and you steer it: you pick the bounty, run the local
grade where there is one, read what the checks say, and decide when to submit. The better you can
judge the work, the better the entries.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo
Code, Aider and dozens more are found by the kit on its own. Other agents can hand over their
session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials,
e-mail addresses and home paths before upload, and `bundle --redact "<text>"` removes any other
personal detail you name.
</details>

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — ask
  questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/bg2sw27v · code <code>bg2sw27v</code></sub>
