# Linux Commands — Class 02 Practice

These notes document my Linux command-line practice from **Module 01, Class 02** of the Ostad Data Engineering Bootcamp. The commands use the original course datasets; raw CSV and log files are not committed because they are excluded by `.gitignore`.

## Dataset setup

From the course repository root (the folder containing `module-01/`):

```bash
cd ~/de-bootcamp/ostad-de-bootcamp-main
ORDERS="module-01/data/orders_oct2026.csv"
LOG="module-01/data/app_2026-10.log"
```

> If your downloaded course repository is in a different folder, change the `cd` path. Run these commands from the course repository, not from `data-engineering-journey`.

## 1. Count lines in the orders CSV

```bash
wc -l "$ORDERS"
```

**Result:** 46,908 lines. This includes **one header row**, so the file contains **46,907 orders**.

## 2. Count lines in the application log

```bash
wc -l "$LOG"
```

**Result:** 100,000 lines.

## 3. Count WARN log entries

```bash
grep -c '| WARN' "$LOG"
```

**Result:** 12,884 WARN entries.

**Why not `| WARN |`?** The log uses a fixed-width level field, so WARN is padded with an extra space: `| WARN  |`.

## 4. Count DEBUG log entries

```bash
grep -c '| DEBUG |' "$LOG"
```

**Result:** 3,983 DEBUG entries.

## 5. Count ERROR entries on October 10, 2026

```bash
grep '^2026-10-10' "$LOG" | grep -c '| ERROR |'
```

**Result:** 149 ERROR entries.

## 6. Find the date with the most WARN entries

```bash
grep '| WARN' "$LOG" | cut -d' ' -f1 | sort | uniq -c | sort -rn | head -1
```

**Result:** 2026-10-17 — 556 WARN entries.

The pipeline filters WARN entries, extracts the date, groups equal dates, counts their occurrences, sorts counts descending, and returns the highest count.

## 7. Count orders paid with bKash

```bash
grep -c 'bKash' "$ORDERS"
```

**Result:** 15,922 orders.

## Bonus 8. Count ERROR entries from the auth component

```bash
grep '| ERROR |' "$LOG" | grep -c '| auth'
```

**Result:** 242 ERROR entries.

## Bonus 9. Find the date with the fewest ERROR entries

```bash
grep '| ERROR |' "$LOG" | cut -d' ' -f1 | sort | uniq -c | sort -n | head -1
```

**Result:** 2026-10-24 — 95 ERROR entries.

## Useful Git commands practiced in Module 02

```bash
git status                   # Check working tree status
git add README.md            # Stage a specific file
git commit -m "Describe work" # Save a meaningful snapshot
git log --oneline            # Review commit history
git switch -c update-readme  # Create and enter a feature branch
git push -u origin main      # Push the main branch to GitHub
```

**Key takeaway:** Small, descriptive commits make the history easy to understand, while branches and pull requests let us review changes before merging them into `main`.
