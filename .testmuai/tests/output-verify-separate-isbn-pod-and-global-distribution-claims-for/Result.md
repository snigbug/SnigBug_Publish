---
test: ../verify-separate-isbn-pod-and-global-distribution-claims-for_test.md
status: passed
started: 2026-09-11T18:24:40.109Z
duration_s: 232
session_id: f11f24bd-c720-414f-ba9c-d49d1e7bd831
---

# Verify separate ISBN, POD, and global distribution claims for self-publishing — Result

## Step 1 ✓ passed (52.7s)
md5: 06989fb839708536f09adb73f12cd1b0
Open https://notionpress.com/ and navigate to the self-publishing page that presents the publishing workflow, format details, and distribution information.

## Step 2 ✓ passed (47.7s)
md5: 23aaa667c0dcb993470b43789503655d
On the self-publishing page's formats and printing sections, review the ISBN and production statements, then assert the page states that each format is issued its own separate ISBN and that printing uses a print-on-demand model driven by sales velocity.

## Step 3 ✓ passed (128.8s)
md5: 2afcc1242966eea724b0cb246a47897a
On the same page's distribution section, review the reach statement, then assert Amazon and the Notion Press Store are named as sales channels and the reach claim shows 30,000+ stores across 150+ countries.
