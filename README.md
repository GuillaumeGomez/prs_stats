## Build data about PRs lifecycle

## Discussion in these Zulip streams:

- [#general > PR tracking](https://rust-lang.zulipchat.com/#narrow/stream/122651-general/topic/PR.20tracking)
- [#t-compiler > review latency meeting planning](https://rust-lang.zulipchat.com/#narrow/stream/131828-t-compiler/topic/review.20latency.20meeting.20planning)
- [#t-compiler > reducing PR review time](https://rust-lang.zulipchat.com/#narrow/stream/131828-t-compiler/topic/Reducing.20PR.20review.20time)

## Requirements && install

Data retrieval and parsing:

- Needs Python 3.x. Create a virtualenv and install the packages:
``` sh
$ virtualenv venv
$ source venv/bin/activate
(venv) $ pip install -r requirements.txt
```

Website presentation:

- Needs npm LTS. This should install the needed packagesa and start server on http://localhost:8000
``` sh
npm run dev
```

- Compile with:

``` sh
sh update_dist.sh
```

Output files are in `./prs_stats`. Test deployed compiled .js with any http local server, ex. `python -m http.server` and then open `http://localhost:8000/prs_stats`.

## Scripts

These scripts will fetch and process the pull requests data. Directory `./src/data` constains all .json files used by the website.

1. Download the entire PRs corpus from [rust-lang/rust](https://github.com/rust-lang/rust/pulls) (+6K pull requests, will take hours! Final dump is about 1GB uncompressed)

``` sh
./get-all-prs.py (with sorting by date) > prs.json
```

2. How long do stay PRs open before being closed?

``` sh
# filters all PRs in Q1/2023
./filter-prs-by-date.py 2023-01-01 2023-04-30 > src/data/t-compiler/2023.json
```

3. How many PRS are against issues?

``` sh
# filters all PRs in Q1/2023
./get-regression-issue-for-prs.py 2023-01-01 2023-04-30 > src/data/t-compiler/2023_contrast_prs_bugfixes.json
```

## Chores

- Compiled .js has zoom gesture and reset button broken, to test  (on local dev env it works)
