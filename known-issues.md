# Known Issues

## Minio

Refer to official documents, <https://docs.minio.io/>.

And open issues <https://github.com/minio/minio/issues>.

## COSBench

1. For JDK deployment, portal to [Oracle official sites](http://www.oracle.com/technetwork/cn/java/javase/downloads/index.html).
    * Linux: download dpk for Debian/Ubuntu, rpm for CentOS, as for the latter, simply run `rpm -ivh <downloaded jdk rpm package>`.
    * Windows: download installation package and run.
    * Some references <https://github.com/Zhan2012/java-bundle>

2. In [COSBench release page](https://github.com/intel-cloud/cosbench/releases), choose release candidate rather than final release (require manual compilation).
    * refer to [open-io's choice](https://github.com/open-io/cosbench/releases).
    * download zip file for running directly without compilation.
    * If you are familiar with Java, clone the repo and build from the latest source for yourself.

3. COSBench scripts
    * Use `start-all.sh` to run and `stop-all.sh` to stop.
    * Use `chmod +x` to set execution property for both scripts.
    * Or simply run `source <above scripts>` or `. <above scripts>`, refer to Linux basics.
    * As for Windows, run start-all.bat instead, no `chmod` or `source` is required.
    * start-all.sh depends on _**nc**_, if its not available
        * Ubuntu: `apt-get install nmap`
        * CentOS: `yum install nmap-ncat`

4. COSBench workload definition
    * "cprefix" naming
        * Avoid special characters in "cprefix", such as '\_' and '-'.
        * Avoid uppercase letters in "cprefix".
    * Value settings
        * Follow page 36-38 "4. Configuring Workloads" strictly, especially the use of various selectors.

5. Understand workload properly
    * init: make buckets/containers for benchmarking (both read and write tests).
    * prepare: write objects for benchmarking (read test).
    * main: default workstage, with work type ommitted, may setup multiple main workstages for various testing.
    * `containers=r(1,2);objects=r(1,8);sizes=c(8)KB`:
        * 2 containers are created (number from 1 to 2).
        * For each container, 8 objects (number from 1 to 8, 8KB each) are written.
    * In current COSBench implementation, 1KB=1000Bytes (-\_-\|\|\|).

6. Understand result properly
    * Succ-Ratio: the ratio of successing requests.
    * "Stat: completed": Succ-Ratio >= 80%
    * "Stat: Failed": Succ-Ratio < 80%
    * For failed requests, check `workload.log` in `cosbench/archive/w[dd]-[workload name]`, usually bad checksum.

## s3proxy

1. To work with latest s3cmd and aws-shell, authorization should be disabled: `s3proxy.authorization=none`
    * No authorization, not for practical use.
    * TODO: make authorization compatible with current s3cmd/aws-shell.

2. Windows: incorrect folder access, 'mb' may work, but with wrong properties, which prevent further operations like cp/put
    * No 'official' support for Windows, possible cause: unsupported file system metadata operation
    * TODO: store metadata separately, like mock-s3.

## mock-s3

1. Python 2.7 restricted.
    * Python 3 migration in progress, maybe BUGGY.

2. No authorization, not for practical use.

3. Windows: incorrect file IO, although CLI-based GET/PUT may work, COSBench will fail on read.

