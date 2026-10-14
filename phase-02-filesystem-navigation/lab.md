# Phase 02 — Hands-On Lab Guide

**Platform:** CentOS Stream 9 / Bash  
**Safety:** Run these labs in your home directory. Do not execute destructive examples on system or company paths.

## Preparation

```bash
mkdir -p ~/linux-for-devops/phase-02
cd ~/linux-for-devops/phase-02
```

## Lab 01 — Navigation and paths

```bash
pwd
cd /etc
pwd
cd /var
pwd
cd ~
pwd
cd -
pwd
```

**Learn:** Absolute paths, home (`~`), previous directory (`cd -`).

## Lab 02 — Directory structure

```bash
cd ~/linux-for-devops/phase-02
mkdir -p devops-demo-app/{config,logs,scripts,backup,deployments}
ls -lah devops-demo-app
```

**Learn:** `mkdir -p`, Bash brace expansion.

## Lab 03 — Create files and write logs

```bash
cd ~/linux-for-devops/phase-02/devops-demo-app
touch application.log config/application.properties logs/app.log scripts/deploy.sh
echo 'Application started successfully' > logs/app.log
echo 'Database connection established' >> logs/app.log
cat logs/app.log
```

**Learn:** `touch`, overwrite (`>`), append (`>>`).

## Lab 04 — Copy, move, rename, delete

```bash
echo 'server.port=8080' > config/application.properties
cp config/application.properties backup/
cp -r config backup/config-backup
mv application.log application-old.log
mv application-old.log logs/
touch test-delete.txt
rm test-delete.txt
mkdir test-empty
rmdir test-empty
ls -lah logs backup
```

**Learn:** `cp`, recursive copy, `mv`, safe deletion of disposable files.

## Lab 05 — Read and follow logs

```bash
printf '%s
' \
 'INFO Application started' \
 'INFO Database connected' \
 'INFO User login successful' \
 'WARN Memory usage increasing' \
 'ERROR Database connection timeout' \
 'ERROR API request failed' > logs/app.log
cat logs/app.log
head -n 3 logs/app.log
tail -n 3 logs/app.log
less logs/app.log
```

To test live following, run `tail -f logs/app.log` in terminal 1. In terminal 2 run:

```bash
echo 'INFO New API request received' >> ~/linux-for-devops/phase-02/devops-demo-app/logs/app.log
```

Stop terminal 1 with `Ctrl+C`.

## Lab 06 — Find files and grep content

```bash
find . -name '*.log'
find . -name 'application.properties'
find . -type d
find . -type f
find . -type f -size +1M
grep 'ERROR' logs/app.log
grep -i 'error' logs/app.log
grep -n 'ERROR' logs/app.log
grep -c 'ERROR' logs/app.log
```

**Expected:** Two `ERROR` lines in the sample log.

## Lab 07 — Pipes

```bash
cat logs/app.log | grep 'ERROR'
grep 'ERROR' logs/app.log | wc -l
```

**Learn:** Standard output piped to standard input. Direct `grep 'ERROR' logs/app.log` is simpler when no `cat` is needed.

## Lab 08 — Wildcards

```bash
touch test1.txt test2.txt test3.txt app1.log app2.log
ls *.txt
ls *.log
ls test?.txt
ls test[0-9].txt
```

**Learn:** `*`, `?`, character classes.

## Lab 09 — File metadata

```bash
stat logs/app.log
file logs/app.log
ls -li logs/app.log
```

**Learn:** File size, timestamps, inode, content type.

## Lab 10 — Hard and symbolic links

```bash
ln logs/app.log logs/app-hardlink.log
ln -s app.log logs/app-symlink.log
ls -li logs/app.log logs/app-hardlink.log
ls -l logs/app-symlink.log
echo 'INFO Link test' >> logs/app.log
tail -n 1 logs/app-hardlink.log
tail -n 1 logs/app-symlink.log
```

**Learn:** Shared inode for hard links; relative target path for symbolic links. These link-creation commands assume links do not already exist; use a clean practice directory if rerunning.

## Lab 11 — Deployment troubleshooting scenario

**Scenario:** Deployment reports `config/application.properties` is missing.

```bash
cd ~/linux-for-devops/phase-02/devops-demo-app
pwd
ls -lah
find . -name 'application.properties'
cat config/application.properties
stat config/application.properties
grep 'server.port' config/application.properties
```

**Questions:** Is the file actually absent? Is the script using the expected working directory? Does the file contain `server.port=8080`? If present but unreadable, what permission checks should follow?

## Final verification

```bash
cd ~/linux-for-devops/phase-02/devops-demo-app
pwd
find . -maxdepth 2 -type f
cat config/application.properties
grep -n 'ERROR' logs/app.log
ls -li logs/app.log logs/app-hardlink.log
ls -l logs/app-symlink.log
```

**Next:** Phase 03 — Users, Groups, Permissions and Ownership.
