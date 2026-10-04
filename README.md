# devops-monitorzombieprocess

A timer checks running processes every 5 minutes. It sends a pid when a long job is busy and its output file has stopped changing.

The message body is the pid only. The target is `NTFY_URL` in `process-guard`.

## Install

```bash
install -m 755 process-guard ~/.local/bin/process-guard
mkdir -p ~/.config/process-guard ~/.config/systemd/user
cp config/allowlist ~/.config/process-guard/allowlist
cp systemd/process-guard.service systemd/process-guard.timer ~/.config/systemd/user/
systemctl --user daemon-reload
systemctl --user enable --now process-guard.timer
```

## Rules

- The process start time is at least 15 minutes ago.
- CPU use is at least 85% of one core, or GPU compute is at least 50%.
- Memory size is ignored.
- The output path in the command is unchanged for 10 minutes.

Add a process name to `config/allowlist`, one name per line, to skip it.

## Check

```bash
process-guard --self-test
process-guard --once
```

`--once` records a sample and prints pids. It does not send an alert.
