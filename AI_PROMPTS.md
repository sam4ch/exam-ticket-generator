# AI-Assisted Development Log

This project was completed with AI assistance, in line with the lab's learning objective.

## 1. Assignment analysis

AI extracted the requirements from the workshop plan PDF and the Word specification. Requirements were grouped into functional, technical, and validation requirements.

## 2. Technology selection

At the student's request, Python, Java, Node.js, Go, and C# were considered. C# / .NET 8 was selected because `Console.ReadKey()` supports handling `ESC`, and the .NET standard library can create an `.xlsx` file without adding a dependency to the main program.

## 3. Architecture prompt

**Goal:** split the application into small, testable components for input, validation, ticket generation, the record model, and the Excel journal.

**Result:** `ConsoleInput`, `InputValidator`, `TicketGenerator`, `StudentRecord`, and `ExcelJournal` were created.

## 4. Implementation prompt

**Requirements given to AI:**

- Read input character by character and handle `Enter`, `Backspace`, and `ESC`.
- Reject empty strings and trim leading/trailing whitespace.
- Generate tickets in the range 1–20.
- Create `journal.xlsx` with the four required columns.
- Safely replace the file after each record while preserving previous rows.
- Handle access errors when the Excel file is open.
- Do not add requirements absent from the original assignment.

## 5. Testing prompt

AI created self-contained unit tests for ticket bounds, input validation, Excel file creation, sequential row appends, continued operation after restarting, and locked-file handling. The test runner has no NuGet dependency because the classroom computer's system TLS authentication prevented restoring xUnit.

## 6. Result validation

The code and tests build without network dependencies. Full validation includes automated unit tests and the five manual scenarios from the original specification. Errors discovered during execution should be recorded in separate Git commits with descriptive messages.

## 7. Lab 2: UI prompt

**Goal:** extend the application with a graphical interface without duplicating Lab 1 business logic.

**Result:** AI created an ASP.NET Core Razor Pages application. It reuses `InputValidator`, `TicketGenerator`, `StudentRecord`, and `ExcelJournal`, and displays a form, the generated ticket, and recent journal rows. The interface supports desktop and mobile screens.

## 8. Selenium automation prompt

AI was given the following scenario:

- Start the web application on an available local port.
- Open a supported browser.
- Check empty-form validation.
- Automatically enter three demonstration students with ordinary names.
- Submit the forms and verify ticket numbers.
- Find the students in the table.
- Independently read the persistent `journal.xlsx` and confirm that records were saved.
- Close the test processes while preserving the created entries.

**Result:** a separate `ExamTicketGenerator.SeleniumTests` project was added. The scenario was successfully checked twice through Opera GX on the student's computer; Microsoft Edge is the fallback when Opera is unavailable.

## 9. AI-assisted diagnostics

Validation identified two external constraints: Windows policy blocked Edge remote debugging, and Opera GX reported one version to its installer while using a different Chromium version. AI determined the actual versions, selected a compatible ChromeDriver from NuGet, and handled the initial GX Corner tab by opening a separate tab for the application under test.

## 10. GitHub email notifications

At the student's request, AI added a GitHub Actions workflow that reacts to `push` and sends an SMTP email containing the repository name, branch, author, commit list, and a link to the changes. Credentials, app passwords, and addresses are not stored in code; they are supplied only through GitHub Actions Secrets. The workflow uses the Python standard library and does not depend on third-party GitHub Actions.
