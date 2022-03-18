# Some onboarding tips

* Learn how to use Loki early -- lots of times, artifact logs are not present (esp. for upgrade tests
  where only logs of the upgrade are present and logs of the initial installation are gone)
  * If you know something about LogQL, it will help a lot
  * If you can get the logcli to work, you can get logs in text vs. in the UI
  * Look for "Debug tools" at the top of a Prow job link to expand and see links for how to start it up
* Learn how to use PromeCIeus early and be able to take queries from Prometheus alarms expressions and
  run them to understand how the alarm happened
  * If you know something about PromQL, it will help a lot
  * Look for "Debug tools" at the top of a Prow job link to expand and see links for how to start it up

* Learn how to navigate [Prow](https://prow.ci.openshift.org/)
  * Learn how to search jobs and PRs so you can look at history of jobs (e.g., to see how long something
    has failed)
  * [Example prow job](https://prow.ci.openshift.org/view/gs/origin-ci-test/logs/periodic-ci-openshift-release-master-nightly-4.11-e2e-aws-upgrade/1504516291879243776)

* Learn where things are in the "artifacts" directories
  * "Artifacts" link is in upper right corner of a prow job
  * This [dir](https://gcsweb-ci.apps.ci.l2s4.p1.openshiftapps.com/gcs/origin-ci-test/logs/periodic-ci-openshift-release-master-nightly-4.11-e2e-aws-upgrade/1504516291879243776/artifacts/e2e-aws-upgrade/) has the `gather-extra` and `gather-must-gather` subdirs.
  * This [dir](https://gcsweb-ci.apps.ci.l2s4.p1.openshiftapps.com/gcs/origin-ci-test/logs/periodic-ci-openshift-release-master-nightly-4.11-e2e-aws-upgrade/1504516291879243776/artifacts/e2e-aws-upgrade/gather-must-gather/artifacts/) has the must-gather.tar file which
    contains a lot of files about a job run; unzip/untar that to see the files
    * Some people like to use [o-must-gather](https://github.com/kxr/o-must-gather) to look at logs
  * This [dir](https://gcsweb-ci.apps.ci.l2s4.p1.openshiftapps.com/gcs/origin-ci-test/logs/periodic-ci-openshift-release-master-nightly-4.11-e2e-aws-upgrade/1504516291879243776/artifacts/e2e-aws-upgrade/openshift-e2e-test/) has build-log.txt which is a very large log that is useful to peruse
  * This [dir](https://gcsweb-ci.apps.ci.l2s4.p1.openshiftapps.com/gcs/origin-ci-test/logs/periodic-ci-openshift-release-master-nightly-4.11-e2e-aws-upgrade/1504516291879243776/artifacts/e2e-aws-upgrade/openshift-e2e-test/artifacts/junit/) has a lot of json and html files.
    * Those html files are for the intervals chart mentioned earlier.  The json files are the json used to render those charts.
    * The interval charts have text cut off so you can't reliably search them via the browser search function so you might want to
      search the json files.
    * If you know how to use jq, this will help a lot
    * A json view like [this](http://jsonviewer.stack.hu/) might help (unless the file is just too huge)
  * Be able to download files with curl -- vs. clicking, waiting for a huge log to render in the browser and copy/pasting huge logs, etc.
    * Get the link and do `curl -s '<the link>' -o <some-output-file-name>`
    * Example that takes a while in browser but seconds on command line:
      * `curl -s 'https://gcsweb-ci.apps.ci.l2s4.p1.openshiftapps.com/gcs/origin-ci-test/logs/periodic-ci-openshift-release-master-nightly-4.11-e2e-aws-upgrade/1504516291879243776/artifacts/e2e-aws-upgrade/openshift-e2e-test/build-log.txt' -o build-log.txt`

* Know how to navigate [step-registry](https://github.com/openshift/release/tree/master/ci-operator/step-registry)
  * This will let you see what is run in sequence
  * Note [this link](https://docs.ci.openshift.org/docs/architecture/step-registry/#registry-layout-and-naming-convention)
    which tells you the naming convention of the parts of a step so you know how to search for it in the [release repo](https://github.com/openshift/release/)
  * At the bottom of Prow jobs, you will see something like (see bottom of [this prow job](https://prow.ci.openshift.org/view/gs/origin-ci-test/logs/periodic-ci-openshift-release-master-nightly-4.11-e2e-aws-upgrade/1504516291879243776)):

  ```
  Link to step on registry info site: https://steps.ci.openshift.org/reference/openshift-e2e-test
  Link to job on registry info site: https://steps.ci.openshift.org/job?org=openshift&repo=release&branch=master&test=e2e-aws-upgrade&variant=nightly-4.11
  ```

  Those links will take you to a rendered step registry where you can navigate (via using more browser tabs) to see what steps are taken in a test and
  the script that is run.  The job link is like a "flat representation of a tree" so you can navigate using browser tabs as you would navigate a tree
  represented as a list (the tree's leaves are the actual (bash) script).  There's also a link to the [release repo](https://github.com/openshift/release/)
  where the code is located.

* Aggregated jobs (composed of 10 jobs)
  * [Example aggregated job](https://prow.ci.openshift.org/view/gs/origin-ci-test/logs/aggregated-aws-sdn-upgrade-4.11-micro-release-openshift-release-analysis-aggregator/1504516300116856832)
  * Suggested workflow:
    * Make one browser tab for the aggregated job
    * Open the aggregation-job-run-summary.html pane
    * make one browser tab for all 10 jobs -- I bring them all up and wait until they are done
    * peruse the failures in each of the 10 tabs (to see if there is commonality, etc.)

* Know these words because they are used often to describe a problem
  * Job run (the prow job; it has a specific Job ID)
  * Job ID (a large unsigned integer that is unique for a Job run)
  * Tests (tests that are run as part of the Prow job)

* The [ci-docs](https://docs.ci.openshift.org/docs/) are helpful
  * There's a search box there but I found that cloning the [ci-docs repo](https://github.com/openshift/ci-docs) repo
    and searching it there is better
  * I setup vscode and search using that (you can use regular expressions, have multiple search windows, etc.)

* Searching code, tools, etc (some of these are optional depending on your workflow)
  * If you can't search code, you are at a huge disadvantage; I highly recommend vscode (what I use) or goland (probably
    no more licenses left).  Be able to navigate definitions of functions, etc. because there is a lot of code to wade
    through to find what you're looking for
  * Unfortunately, the code is the documentation for the code so learning go will allow you to read the docs more
    effectively.
  * look into [fzf](https://vimawesome.com/plugin/fzf#using-git) for "fuzzy" searching of files (and previously typed
    command)
      * There is so much code in so many subdirectories, it's hard to keep cd'ing everywhere; with fzf, you can find
        what you need and then have the subdir right there
      * You can also "fuzzy" search in vscode for files so you don't have to click directories to navigate to files
        deeply nested in directories
  * learn about git lens in vscode (you can jump to the git history (like who wrote the code, when, and the PR) of any line of code)
  * For commands you need to type a lot (esp. long ones), look into [espanso](https://espanso.org/)(free)
    or [textexpander](https://textexpander.com/)(yearly fee).
  * If you don't know how to use gpg, learn how to create your key, encrypt and decrypt.  Credentials around here are
    passed using gpg encrytion.  See [GNU Privacy handbook](https://www.gnupg.org/gph/en/manual/x56.html).
  * Adjust your google meet settings esp. for recording things.  Unfortunately, the max resolution is 720 so you will
    need to increase font size if you want to record.
  * Know how to squash a PR; the first one in [this](https://gist.github.com/DennisPeriquet/d97317e21388cc61525a994b4ebbe663) is what I
    have used successfully.

* Laptops
  * The Apple Mac (esp. if you can get the bigger screen) is elegant and stylish but if you want to do real Linux stuff, you'll
    need containers and/or VMs.
  * The Lenovo is less elegant (imprecise trackpad, minimal gestures, spartan in nature) but can serve as a decent
    workhorse esp. if like/need to do Linux stuff.
