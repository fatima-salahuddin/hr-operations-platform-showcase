<h1>HR and Operations Platform</h1>

<p>
  <img src="https://img.shields.io/badge/case_study-0F172A?style=flat-square" alt="Case study">
  <img src="https://img.shields.io/badge/status-in_production-0D9488?style=flat-square" alt="In production">
  <img src="https://img.shields.io/badge/Laravel_11-FF2D20?style=flat-square&logo=laravel&logoColor=white" alt="Laravel">
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL">
  <img src="https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB" alt="React">
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker">
  <img src="https://img.shields.io/badge/JWT-000000?style=flat-square&logo=jsonwebtokens&logoColor=white" alt="JWT">
</p>

<table>
  <tr>
    <td align="center"><b>14</b><br><sub>modules</sub></td>
    <td align="center"><b>~140</b><br><sub>API routes</sub></td>
    <td align="center"><b>3 years</b><br><sub>of Excel history migrated<br>and reconciled</sub></td>
    <td align="center"><b>27</b><br><sub>data models</sub></td>
    <td align="center"><b>1</b><br><sub>developer</sub></td>
  </tr>
</table>

An HR, payroll and operations platform I designed and built, as the sole developer, for a manpower
company that supplies staff to oilfield-services clients in Iraq. It replaced a set of Excel master
files and paper slips with one system covering employees, attendance, payroll, invoicing, expenses,
travel and service tickets, documents and profit and loss.

> The source code belongs to the client and the system holds employee and payroll records, so this
> repository holds the write-up only. I am happy to walk through the code on a call.

## At a glance

| | |
|---|---|
| Role | Sole developer: requirements with operations and finance staff, design, build, deployment, support |
| Size | 14 modules, 27 models, 47 migrations, ~140 API routes |
| Backend | Laravel 11 (PHP 8.2), PostgreSQL, JWT auth (tymon/jwt-auth), Spatie Activity Log and Query Builder |
| Documents | DomPDF salary slips and invoices, PhpSpreadsheet imports and exports, Cloudinary + Intervention Image |
| Frontend | React, TanStack Query, React Hook Form + Zod, Radix UI, Recharts, react-pdf, Framer Motion |
| Hosting | Docker image deployed to Railway with PostgreSQL and migrations run on each release |

## Architecture

```mermaid
flowchart LR
    XLS["Excel master files\n(2023 onward)"] -- "one-off migration\n+ reconciliation" --> API
    WEB["React web app"] <-- "JWT" --> API["Laravel 11 REST API\n(Docker on Railway)"]
    API <--> DB[("PostgreSQL")]
    API -- "photos, documents" --> CDN["Cloudinary"]
    API -- "salary slips,\ninvoices" --> PDF["PDF output"]
    API -- "every change" --> LOG["Audit log"]
```

## Modules

- **Employees:** profiles, bank accounts, department and designation history, rate history, documents
- **Attendance:** monthly attendance per employee and site, feeding payroll directly
- **Payroll:** salary calculation from attendance and the rate in force for that month, advances,
  held salaries, arrears, PDF salary slips and slip payments
- **Invoicing:** client invoices with line items built from the same attendance, partial payments
- **Expenses, air tickets, service tickets:** operational costs tracked per employee and department
- **Profit and loss:** monthly P&L per department, plus partner profit distribution
- **Documents:** uploads to Cloudinary with image processing
- **Users, roles and audit:** JWT role-based access, admin user management, and an activity log of every
  create, update and delete across modules
- **Reports and settings**

## Problems worth talking about

**Three years of history from spreadsheets.** The company had run on monthly master workbooks since
2023, with salary sheets, invoices and P&L in different files and formats. I wrote import scripts for
the real workbooks, loaded the history into PostgreSQL, then produced comparison sheets (system vs.
original sheet) for payslips, gross salary and profit, month by month, until the differences were
explained or fixed. The finance team signed off on numbers they could trace back to their own files.

**Rates that change over time.** Employees move between departments and get rate changes mid-year, and
old payslips must still reproduce exactly. Rates and departments are stored as dated history rows, and
payroll always reads the rate that applied in that month instead of the current one.

**Payroll edge cases as data, not hacks.** Advances, held salaries and arrears are their own records
linked to the slip they affect, so a held salary released three months later shows up correctly on
both slips and in the P&L.

**One source for payroll and invoicing.** The same attendance record drives what the employee is paid
and what the client is billed, so the two can no longer drift apart.

**Accountability.** Every change is written to an audit log with the user and the before and after
values, which matters when salaries and client invoices are involved.

## Contact

Fatima Salahuddin, BS Computer Science, FAST-NUCES Karachi
[Profile](https://github.com/fatima-salahuddin) · [LinkedIn](https://www.linkedin.com/in/fatima-salahuddin-/) · fatimasalahuddinmirza@gmail.com
