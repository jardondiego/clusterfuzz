# ClusterFuzz

<p align="center">
  <img src="docs/images/logo.png" width="400">
</p>

[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/google/clusterfuzz/badge)](https://api.securityscorecards.dev/projects/github.com/google/clusterfuzz)

ClusterFuzz is a scalable [fuzzing](https://en.wikipedia.org/wiki/Fuzzing)
infrastructure that finds security and stability issues in software.

Google uses ClusterFuzz to fuzz all Google products and as the fuzzing
backend for [OSS-Fuzz].

ClusterFuzz provides many features which help seamlessly integrate fuzzing into
a software project's development process:
- Highly scalable. Can run on any size cluster (e.g. OSS-Fuzz instance runs on
  100,000 VMs).
- Accurate deduplication of crashes.
- Fully automatic bug filing, triage and closing for various issue trackers
  (e.g. [Monorail], [Jira]).
- Supports multiple [coverage guided fuzzing engines]
  ([libFuzzer], [AFL], [AFL++] and [Honggfuzz])
  for optimal results (with [ensemble fuzzing] and [fuzzing strategies]).
- Support for [blackbox fuzzing].
- Testcase minimization.
- Regression finding through [bisection].
- Statistics for analyzing fuzzer performance, and crash rates.
- Easy to use web interface for management and viewing crashes.
- Support for various authentication providers using [Firebase].

## Overview

<p align="center">
  <img src="docs/images/overview.png">
</p>

## Documentation
You can find detailed documentation [here](https://google.github.io/clusterfuzz).

## Trophies
As of February 2023, ClusterFuzz has found ~27,000 bugs in Google (e.g. [Chrome]). Additionally, ClusterFuzz has helped identify and fix over [8,900] vulnerabilities and [28,000] bugs across [850] projects integrated with [OSS-Fuzz].

## Getting Help
You can [file an issue](https://github.com/google/clusterfuzz/issues/new) to ask
questions, request features, or ask for help.

## Staying Up to Date
We will use [clusterfuzz-announce(#)googlegroups.com](https://groups.google.com/forum/#!forum/clusterfuzz-announce) to make announcements about ClusterFuzz.

## ClusterFuzzLite
For a more lightweight version of ClusterFuzz that runs on CI/CD
systems, check out [ClusterFuzzLite](http://github.com/google/clusterfuzzlite).

## Butler

`butler.py` is a script to help you with command-line tasks (e.g. running unit tests, deploying). It is the main tool for developing and maintaining ClusterFuzz.

Here is what it can do:

- **bootstrap**: Installs all required dependencies (Python, Go, etc.) for running the App Engine, bots, and MapReduce locally.
- **py_unittest**: Runs Python unit tests. Supports running tests in parallel, filtering by pattern, and targeting specific modules (appengine, core, modules, cli).
- **js_unittest**: Runs JavaScript unit tests. Supports keeping the browser open for debugging.
- **format**: Formats changed code in the current branch using standard formatters.
- **lint**: Lints changed code in the current branch. Can also run type checkers.
- **run_server**: Runs the local ClusterFuzz server. Supports bootstrapping the local database and skipping dependency installation.
- **run_bot**: Runs a local ClusterFuzz bot. You can specify the bot name and directory.
- **reproduce**: Reproduces a testcase locally. Requires a config directory and a testcase ID.
- **deploy**: Deploys ClusterFuzz to App Engine. Supports deploying to staging or production, and targeting specific components (appengine, terraform, zips).
- **package**: Packages ClusterFuzz source code into a zip file for staging or release.
- **create_config**: Creates a new deployment configuration, setting up project ID, Firebase API key, OAuth secrets, and region settings.
- **integration_tests**: Runs end-to-end integration tests to ensure system stability.
- **remote**: Helper to run commands on a remote bot instance. Supports tailing logs (`tail`, `tailf`), restarting the bot, rebooting the instance, staging zip files, and launching RDP.
- **run**: Runs a one-off script (from `local/butler/scripts`) against the datastore. Useful for migrations or maintenance tasks.
- **clean_indexes**: Cleans up undefined indexes in `index.yaml` based on the current configuration.
- **weights**: Command to interact with probability weights for fuzzers and jobs, allowing you to list, aggregate, or set weights for fuzzing scheduling.

[Chrome]: https://bugs.chromium.org/p/chromium/issues/list?can=1&q=label%3AClusterFuzz+-status%3AWontFix%2CDuplicate
[8,900]: https://bugs.chromium.org/p/oss-fuzz/issues/list?q=status%3AFixed%2CVerified%20Type%3DBug-Security&can=1
[28,000]: https://bugs.chromium.org/p/oss-fuzz/issues/list?q=status%3AFixed%2CVerified%20Type%3DBug&can=1
[850]: https://github.com/google/oss-fuzz/tree/master/projects
[OSS-Fuzz]: https://github.com/google/oss-fuzz
[Monorail]: https://opensource.google.com/projects/monorail
[Jira]: https://www.atlassian.com/software/jira
[bisection]: https://en.wikipedia.org/wiki/Bisection_(software_engineering)
[Firebase]: https://firebase.google.com/docs/auth
[libFuzzer]: http://llvm.org/docs/LibFuzzer.html
[AFL]: https://github.com/google/AFL
[AFL++]: https://github.com/AFLplusplus/AFLplusplus
[Honggfuzz]: https://github.com/google/honggfuzz
[blackbox fuzzing]: https://google.github.io/clusterfuzz/setting-up-fuzzing/blackbox-fuzzing/
[coverage guided fuzzing engines]: https://google.github.io/clusterfuzz/setting-up-fuzzing/libfuzzer-and-afl/
[fuzzing strategies]: https://i.blackhat.com/eu-19/Wednesday/eu-19-Arya-ClusterFuzz-Fuzzing-At-Google-Scale.pdf#page=27
[ensemble fuzzing]: https://www.usenix.org/system/files/sec19-chen-yuanliang.pdf
