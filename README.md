# Smart Roster Management System

Datastraw Tech Internship Assessment, by Tanushri Shelke.

- **Live app:** `https://shelketanushri5-dev.github.io/smart-roster/`
- **Demo video:** 'https://drive.google.com/file/d/17Vqobvryt6QgsaPxplrdcMK-HMd45JbQ/view?usp=drivesdk'
- **LinkedIn:** (https://www.linkedin.com/in/tanushri-shelke-644b5b451?utm_source=share_via&utm_content=profile&utm_medium=member_android)

A browser-based tool that helps an operations manager plan, generate, validate and change monthly rosters across brands, processes and pods. It comes with a realistic sample dataset (32 employees, 27 brands, 4 pods).

## Run it

No setup needed. It is a single `index.html` with plain HTML, CSS and JavaScript. Open the file in a browser, or use the live link above. Data is saved in the browser (localStorage). "Reset sample" restores the sample data. No `.env` file is needed.

## How to use

1. **Roster tab:** choose a month, then click *Generate roster*. Use the dropdowns for company, pod, brand, process or employee views. Tap a cell to force OFF or WORK and see the impact. The coverage grid shows working / required for each process (green = ok, red = gap).
2. **Conflicts & changes tab:** shows all gaps with reasons and suggested backup employees. Add a leave or comp-off here and the roster recalculates instantly.
3. **Setup tab:** add or edit employees, pods, pod leads, brands, processes, calendars, minimum staffing, assignments, leaves and public holidays. No code changes are needed.

## Assumptions

- Each employee gets one weekly off, chosen by the generator unless a fixed off is set.
- Process calendars: 365 days, daily except public holidays, or Sundays and public holidays off.
- Minimum staffing can differ for weekdays and for Saturday/Sunday.
- If none of an employee's processes operate on a day, the day shows as closed.
- Leave and comp-off always override the schedule.
- Mandatory rules are coverage, fixed offs and leaves. Preferred offs are soft.
- Public holidays in the sample data are fictional.

## Tell Us How You Think

**1. How did you model employees, brands, processes, pods and their relationships?**
Pods have a lead. Employees belong to a pod. Brands contain processes, and each process has its own calendar and minimum staffing (with a separate minimum for Sat/Sun). Employees and processes are linked through assignments (many-to-many), so one employee can work on many processes and brands. A brand's team is worked out from its processes. Leaves, comp-offs and holidays are separate lists.

**2. How does your system generate a roster and check the constraints?**
For each employee it scores the 7 weekdays and picks the best weekly off. A day scores badly if it would leave one of their processes short, and well if it spreads offs across the team or matches their preference. Then, for every process on every open day, it counts the working people and compares with the minimum. Anything below the minimum is a conflict.

**3. How did you handle single-resource brands vs brands with multiple employees?**
For a single-employee brand, the generator picks a day when the brand is closed (like Sunday) for the weekly off. If the brand runs 365 days, a gap is unavoidable, so it is flagged instead of hidden. For multi-employee brands, weekly offs are staggered so the minimum stays covered.

**4. What happens when an employee who manages all processes for a brand is unavailable on a required day?**
The day turns red with the reason, for example "Kabir (leave)". The system suggests up to 3 free employees (same pod first, lightest workload). The manager picks one as a backup.

**5. How does it handle conflicting requirements or insufficient staffing?**
It shows "X of Y staffed" with the names of the unavailable people. Mandatory rules (coverage, fixed offs, leaves) are errors. Preferences are only warnings. It never silently produces an invalid roster.

**6. Which parts did you automate and which did you leave under human control? Why?**
Automated: weekly-off selection, coverage checks, conflict detection and backup suggestions. Human control: approving leaves, choosing the backup, and manual OFF/WORK overrides. A wrong automatic staffing decision affects real people and clients, so the manager decides.

**7. If the company doubled its team and brand count, what would you change?**
Move data from browser storage to a database with a backend and user logins. Replace the greedy weekly-off logic with an optimiser (for example Google OR-Tools) that plans the whole month together. Add pagination for the large grids.

**8. With one more week, what would you improve first?**
First, automatic regeneration after approved leave, with fair distribution of weekly offs. Second, checks for overlapping responsibilities and workload per employee. Third, a roster approval and publish flow for pod leads, plus automated tests for the rules.

## Known limitations

- Weekly offs are chosen greedily, not by a global optimiser.
- No login, no multi-user sync, no notifications.
- Overlapping-shift checks are not implemented.
