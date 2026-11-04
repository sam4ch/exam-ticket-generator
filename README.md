<div align="center">

# 🎫 Exam Ticket Generator

An exam ticket generator with console and web interfaces, an Excel journal, and Selenium automation.

![C#](https://img.shields.io/badge/C%23-12-512BD4?logo=csharp&logoColor=white)
![.NET](https://img.shields.io/badge/.NET-8.0-512BD4?logo=dotnet&logoColor=white)
![ASP.NET Core](https://img.shields.io/badge/ASP.NET_Core-Razor_Pages-512BD4?logo=dotnet&logoColor=white)
![Selenium](https://img.shields.io/badge/Selenium-4.49-43B02A?logo=selenium&logoColor=white)
![Tests](https://img.shields.io/badge/unit_tests-8%2F8-brightgreen)
![Labs](https://img.shields.io/badge/labs-1_%26_2-completed-success)

</div>

## About

A university project focused on AI-assisted development. Lab 1 introduced the console application; Lab 2 added a responsive ASP.NET Core web interface and an end-to-end Selenium test.

The user enters a student's last and first names. The application generates a ticket number from **1 to 20**, displays the result, and appends a record to `journal.xlsx`. Previous rows are preserved across runs.

## Features

| Feature | Lab | Implementation |
|---|---:|---|
| Name entry and validation | 1 | Rejects empty values and trims leading/trailing whitespace |
| Ticket generation | 1 | Random integer from 1 through 20, inclusive |
| Excel journal | 1 | A genuine `.xlsx` file updated after each student |
| Persistent history | 1 | New rows do not overwrite previous entries |
| Console exit | 1 | Press `ESC` while entering a last or first name |
| Responsive UI | 2 | Razor Pages form, result, errors, and journal table |
| Selenium | 2 | Automated student entry and form submission |
| Excel E2E validation | 2 | Confirms that the student was saved to `.xlsx` |
| Browsers | 2 | Opera GX, with Microsoft Edge as the fallback |

## Quick start

Open the folder containing `ExamTicketGenerator.sln`, `global.json`, `src`, and `tests` in VS Code. Run all commands below from that repository root.

If your terminal is in the existing local `console app` folder, enter the nested project first:

```powershell
cd ".\ExamTicketGenerator"
```

Restore dependencies and build the solution:

```powershell
dotnet restore
dotnet build
```

The project uses .NET 8; a compatible SDK version is pinned in `global.json`.

## Lab 2: web interface

```powershell
dotnet run --project ".\src\ExamTicketGenerator.Web\ExamTicketGenerator.Web.csproj"
```

Open the URL printed in the terminal, such as `http://localhost:5129`. The interface lets you enter a student, generate a ticket, view recent journal entries, and see validation messages.

The web journal is created at `src/ExamTicketGenerator.Web/journal.xlsx`. Set the `JournalPath` environment variable to use a different location.

## Selenium: automated student entry

```powershell
dotnet run --project ".\tests\ExamTicketGenerator.SeleniumTests\ExamTicketGenerator.SeleniumTests.csproj"
```

The test automatically:

1. Starts the web application on an available local port.
2. Opens Opera GX, or Edge if Opera GX is unavailable.
3. Checks empty-form validation.
4. Enters three demonstration students in sequence: Popescu Ana, Ionescu Mihai, and Rusu Elena.
5. Submits each form and verifies that the ticket is within 1–20.
6. Finds all students in the UI table.
7. Reads the persistent Excel journal and confirms that the entries were saved.
8. Closes the browser and server while retaining the added records.

Successful output, with the application's message translated into English:

```text
Browser: Opera GX
SELENIUM PASS: 3 students added through the UI and saved in the persistent Excel journal.
Journal: C:\...\src\ExamTicketGenerator.Web\journal.xlsx
```

A `ChromeDriver` version compatible with the installed Opera GX is supplied as a NuGet package in the test project.

Each Selenium run appends three more rows. By default, records remain in `src/ExamTicketGenerator.Web/journal.xlsx`; set `SELENIUM_JOURNAL_PATH` to select another file.

## Lab 1: console

```powershell
dotnet run --project ".\src\ExamTicketGenerator\ExamTicketGenerator.csproj"
```

Example interaction, with application messages translated into English:

```text
Exam Ticket Generator
Press ESC to exit.
Last name: Popescu
First name: Ana
Ticket No. 7.
Record saved to journal.xlsx.
```

## Excel journal

| Last name | First name | Ticket number | Date and time |
|---|---|---:|---|
| Popescu | Ana | 7 | 2026-09-22 15:32:28 |
| Ionescu | Mihai | 12 | 2026-09-22 15:35:10 |

`journal.xlsx` is listed in `.gitignore`, so student records are not uploaded to GitHub.

## Testing

Unit tests:

```powershell
dotnet run --project ".\tests\ExamTicketGenerator.Tests\ExamTicketGenerator.Tests.csproj"
```

```text
Tests: 8, Passed: 8, Failed: 0
```

Full validation:

```powershell
dotnet build
dotnet run --project ".\tests\ExamTicketGenerator.Tests\ExamTicketGenerator.Tests.csproj"
dotnet run --project ".\tests\ExamTicketGenerator.SeleniumTests\ExamTicketGenerator.SeleniumTests.csproj"
```

## Solution structure

```text
exam-ticket-generator/
├── src/
│   ├── ExamTicketGenerator/                  # core logic and console
│   │   ├── InputValidator.cs
│   │   ├── TicketGenerator.cs
│   │   ├── StudentRecord.cs
│   │   └── ExcelJournal.cs
│   └── ExamTicketGenerator.Web/              # ASP.NET Core UI
│       ├── Pages/Index.cshtml
│       ├── Pages/Index.cshtml.cs
│       └── wwwroot/css/site.css
├── tests/
│   ├── ExamTicketGenerator.Tests/            # unit tests
│   └── ExamTicketGenerator.SeleniumTests/    # browser E2E test
├── AI_PROMPTS.md                            # AI development log
├── ExamTicketGenerator.sln
└── README.md
```

## Architecture

| Component | Responsibility |
|---|---|
| `InputValidator` | Cleans and validates first and last names |
| `TicketGenerator` | Generates a ticket from 1 to 20 |
| `StudentRecord` | Models a journal row |
| `ExcelJournal` | Creates, reads, and safely updates `.xlsx` files |
| `ExamTicketGenerator.Web` | Lab 2 web interface |
| `ExamTicketGenerator.SeleniumTests` | End-to-end automation of user actions |

The console and web interface share the same business logic and journal format.

## Use of AI

AI assisted with assignment analysis, stack selection, design, C# implementation, UI, Excel format, unit and Selenium tests, browser diagnostics, and documentation.

See the detailed development log in [`AI_PROMPTS.md`](AI_PROMPTS.md).

## Commit email notifications

The `.github/workflows/email-on-push.yml` workflow runs after each `push` to any branch and emails the commit list, author, branch, and a link to the changes. A local `git commit` without `git push` does not trigger an email because GitHub has not received the changes yet.

In the GitHub repository, open **Settings → Secrets and variables → Actions → New repository secret** and add:

| Secret | Value |
|---|---|
| `SMTP_HOST` | The mail provider's SMTP server, for example `smtp.gmail.com` |
| `SMTP_PORT` | Usually `587`; SMTP SSL may use `465` |
| `SMTP_USERNAME` | The sender's full email address |
| `SMTP_PASSWORD` | An app password, rather than the account's main password |
| `MAIL_TO` | Optional recipient address; defaults to `SMTP_USERNAME` |
| `MAIL_FROM` | Optional sender address; defaults to `SMTP_USERNAME` |

After adding the secrets, open **Actions**, select **Email notification on push**, and click **Run workflow** to send a test email. Secrets are not stored in the repository or printed in logs.

## Troubleshooting

### The specified project path does not exist

Enter the folder containing `ExamTicketGenerator.sln` and run the command again.

### `journal.xlsx` is open in Excel

Close the workbook and retry saving: Excel locks an open file.

### Incompatible ChromeDriver version

Opera GX has updated its embedded Chromium version. Update the package:

```powershell
dotnet add ".\tests\ExamTicketGenerator.SeleniumTests\ExamTicketGenerator.SeleniumTests.csproj" package Selenium.WebDriver.ChromeDriver
dotnet restore
```

### Edge blocks remote debugging

On some classroom computers, Windows policy prevents WebDriver from controlling Edge. The test automatically uses Opera GX when it is installed.

---

<div align="center">

University project · C# · .NET 8 · ASP.NET Core · Selenium · AI-assisted development

</div>
