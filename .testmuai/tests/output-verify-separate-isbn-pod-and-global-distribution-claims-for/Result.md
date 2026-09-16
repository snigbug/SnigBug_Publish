---
test: ../verify-separate-isbn-pod-and-global-distribution-claims-for_test.md
status: failed
started: 2026-09-16T04:19:53.638Z
duration_s: 184
session_id: 02d7fb49-1803-4543-a96c-64b700d81595
---

# Verify separate ISBN, POD, and global distribution claims for self-publishing — Result

## Step 1 ✓ passed (28.5s)
md5: 06989fb839708536f09adb73f12cd1b0
Open https://notionpress.com/ and navigate to the self-publishing page that presents the publishing workflow, format details, and distribution information.

## Step 2 ✓ passed (35.1s)
md5: 23aaa667c0dcb993470b43789503655d
On the self-publishing page's formats and printing sections, review the ISBN and production statements, then assert the page states that each format is issued its own separate ISBN and that printing uses a print-on-demand model driven by sales velocity.

## Step 3 ✗ failed (117.9s)
md5: 2afcc1242966eea724b0cb246a47897a
Reason: AP determined agent is stuck — no viable actions remain — bug verdict: Agent stopped before completing distribution-content review [automation_bug/agent_misstep, confidence 0.92]
On the same page's distribution section, review the reach statement, then assert Amazon and the Notion Press Store are named as sales channels and the reach claim shows 30,000+ stores across 150+ countries.
