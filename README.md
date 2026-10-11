# Practical-7-.net

# .NET Technologies – Practical 7

## Feedback Management Module

**Subject:** .NET Technologies  
**Practical No.:** 7  
**Aim:** Develop Feedback Management Module using MVC Layout, Partial View & Validation.

---

## Student Details

| Field | Details |
|---|---|
| **Name** | Rachana A Shetty |
| **Enrolment No.** | 92400120547 |
| **Class** | 5EV7 |
| **Lab Batch** | B |

---

## Aim

Develop a **Feedback Management Module** using ASP.NET Core MVC with:

- MVC Architecture
- Model
- Controller
- View
- Layout
- Partial View
- HTML Helpers
- Model Validation
- Anti-Forgery Token
- Bootstrap UI

---

## Technologies Used

- C#
- ASP.NET Core MVC
- .NET 8
- Razor Views
- HTML Helpers
- Data Annotations
- Bootstrap 5
- jQuery Validation
- Git
- GitHub

---

## Project Structure

```text
FeedbackManagement/
│
├── Controllers/
│   └── FeedbackController.cs
│
├── Models/
│   └── Feedback.cs
│
├── Views/
│   ├── Feedback/
│   │   ├── Index.cshtml
│   │   └── _FeedbackForm.cshtml
│   │
│   ├── Shared/
│   │   ├── _Layout.cshtml
│   │   ├── _ValidationScriptsPartial.cshtml
│   │   └── Error.cshtml
│   │
│   ├── _ViewImports.cshtml
│   └── _ViewStart.cshtml
│
├── wwwroot/
│   ├── css/
│   │   └── site.css
│   │
│   └── js/
│       └── site.js
│
├── Program.cs
├── FeedbackManagement.csproj
└── README.md
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllersWithViews();

var app = builder.Build();

if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Feedback/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();

app.UseStaticFiles();

app.UseRouting();

app.UseAuthorization();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Feedback}/{action=Index}/{id?}");

app.Run();
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>

</Project>
using FeedbackManagement.Models;
using Microsoft.AspNetCore.Mvc;

namespace FeedbackManagement.Controllers
{
    public class FeedbackController : Controller
    {
        [HttpGet]
        public IActionResult Index()
        {
            return View(new Feedback());
        }

        [HttpPost]
        [ValidateAntiForgeryToken]
        public IActionResult Index(Feedback feedback)
        {
            if (ModelState.IsValid)
            {
                ViewBag.Message = "Feedback submitted successfully!";

                return View(new Feedback());
            }

            return View(feedback);
        }

        [HttpGet]
        public IActionResult Error()
        {
            return View("Error");
        }
    }
}

using System.ComponentModel.DataAnnotations;

namespace FeedbackManagement.Models
{
    public class Feedback
    {
        [Required(ErrorMessage = "Name is required")]
        [Display(Name = "Name")]
        public string Name { get; set; } = string.Empty;

        [Required(ErrorMessage = "Email is required")]
        [EmailAddress(ErrorMessage = "Enter a valid email")]
        [Display(Name = "Email")]
        public string Email { get; set; } = string.Empty;

        [Range(1, 5, ErrorMessage = "Select a rating from 1 to 5")]
        [Display(Name = "Rating")]
        public int Rating { get; set; }

        [Required(ErrorMessage = "Comments are required")]
        [StringLength(
            500,
            MinimumLength = 5,
            ErrorMessage = "Comments must be between 5 and 500 characters"
        )]
        [Display(Name = "Comments")]
        public string Comments { get; set; } = string.Empty;
    }
}
@using FeedbackManagement
@using FeedbackManagement.Models
@using Microsoft.AspNetCore.Mvc
@using Microsoft.AspNetCore.Mvc.Rendering
@using Microsoft.AspNetCore.Mvc.ViewFeatures

@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers

@using FeedbackManagement
@using FeedbackManagement.Models
@using Microsoft.AspNetCore.Mvc
@using Microsoft.AspNetCore.Mvc.Rendering
@using Microsoft.AspNetCore.Mvc.ViewFeatures

@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@{
    Layout = "_Layout";
}

<!DOCTYPE html>
<html lang="en">

<head>

    <meta charset="utf-8" />

    <meta name="viewport"
          content="width=device-width, initial-scale=1.0" />

    <title>
        @ViewData["Title"] - Feedback Management
    </title>

    <link rel="stylesheet"
          href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" />

    <link rel="stylesheet"
          href="~/css/site.css"
          asp-append-version="true" />

</head>

<body>

    <header>

        <nav class="navbar navbar-expand-lg navbar-dark bg-primary shadow">

            <div class="container">

                <a class="navbar-brand fw-bold"
                   asp-controller="Feedback"
                   asp-action="Index">

                    Feedback Management

                </a>

                <button class="navbar-toggler"
                        type="button"
                        data-bs-toggle="collapse"
                        data-bs-target="#navbarContent"
                        aria-controls="navbarContent"
                        aria-expanded="false"
                        aria-label="Toggle navigation">

                    <span class="navbar-toggler-icon"></span>

                </button>

                <div class="collapse navbar-collapse"
                     id="navbarContent">

                    <ul class="navbar-nav ms-auto">

                        <li class="nav-item">

                            <a class="nav-link"
                               asp-controller="Feedback"
                               asp-action="Index">

                                Feedback

                            </a>

                        </li>

                    </ul>

                </div>

            </div>

        </nav>

    </header>

    <main class="container py-4">

        @RenderBody()

    </main>

    <footer class="border-top mt-5 py-3 text-muted">

        <div class="container text-center">

            &copy; 2026 - Feedback Management

        </div>

    </footer>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js">
    </script>

    <script src="~/js/site.js"
            asp-append-version="true">
    </script>

    @await RenderSectionAsync("Scripts", required: false)

</body>

</html>
<!DOCTYPE html>
<html lang="en">

<head>

    <meta charset="utf-8" />

    <meta name="viewport"
          content="width=device-width, initial-scale=1.0" />

    <title>
        @ViewData["Title"] - Feedback Management
    </title>

    <link rel="stylesheet"
          href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" />

    <link rel="stylesheet"
          href="~/css/site.css"
          asp-append-version="true" />

</head>

<body>

    <header>

        <nav class="navbar navbar-expand-lg navbar-dark bg-primary shadow">

            <div class="container">

                <a class="navbar-brand fw-bold"
                   asp-controller="Feedback"
                   asp-action="Index">

                    Feedback Management

                </a>

                <button class="navbar-toggler"
                        type="button"
                        data-bs-toggle="collapse"
                        data-bs-target="#navbarContent"
                        aria-controls="navbarContent"
                        aria-expanded="false"
                        aria-label="Toggle navigation">

                    <span class="navbar-toggler-icon"></span>

                </button>

                <div class="collapse navbar-collapse"
                     id="navbarContent">

                    <ul class="navbar-nav ms-auto">

                        <li class="nav-item">

                            <a class="nav-link"
                               asp-controller="Feedback"
                               asp-action="Index">

                                Feedback

                            </a>

                        </li>

                    </ul>

                </div>

            </div>

        </nav>

    </header>

    <main class="container py-4">

        @RenderBody()

    </main>

    <footer class="border-top mt-5 py-3 text-muted">

        <div class="container text-center">

            &copy; 2026 - Feedback Management

        </div>

    </footer>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js">
    </script>

    <script src="~/js/site.js"
            asp-append-version="true">
    </script>

    @await RenderSectionAsync("Scripts", required: false)

</body>

</html>
@model FeedbackManagement.Models.Feedback

@{
    ViewData["Title"] = "Feedback";
}

<div class="row justify-content-center">

    <div class="col-md-8 col-lg-7">

        <div class="card shadow feedback-card">

            <div class="card-header bg-primary text-white text-center">

                <h2 class="mb-0">
                    Feedback Management
                </h2>

                <p class="mb-0 mt-1">
                    We value your feedback
                </p>

            </div>

            <div class="card-body p-4">

                @if (ViewBag.Message != null)
                {
                    <div class="alert alert-success alert-dismissible fade show"
                         role="alert">

                        <strong>Success!</strong>
                        @ViewBag.Message

                        <button type="button"
                                class="btn-close"
                                data-bs-dismiss="alert"
                                aria-label="Close">
                        </button>

                    </div>
                }

                <partial name="_FeedbackForm"
                         model="Model" />

            </div>

        </div>

    </div>

</div>

@section Scripts
{
    <partial name="_ValidationScriptsPartial" />
}
@model FeedbackManagement.Models.Feedback

<form asp-controller="Feedback"
      asp-action="Index"
      method="post">

    @Html.AntiForgeryToken()

    <div asp-validation-summary="ModelOnly"
         class="text-danger mb-3">
    </div>

    <!-- Name -->

    <div class="mb-3">

        @Html.LabelFor(
            model => model.Name,
            new
            {
                @class = "form-label fw-semibold"
            }
        )

        @Html.TextBoxFor(
            model => model.Name,
            new
            {
                @class = "form-control",
                placeholder = "Enter your name"
            }
        )

        @Html.ValidationMessageFor(
            model => model.Name,
            "",
            new
            {
                @class = "text-danger"
            }
        )

    </div>


    <!-- Email -->

    <div class="mb-3">

        @Html.LabelFor(
            model => model.Email,
            new
            {
                @class = "form-label fw-semibold"
            }
        )

        @Html.TextBoxFor(
            model => model.Email,
            new
            {
                @class = "form-control",
                type = "email",
                placeholder = "Enter your email"
            }
        )

        @Html.ValidationMessageFor(
            model => model.Email,
            "",
            new
            {
                @class = "text-danger"
            }
        )

    </div>


    <!-- Rating -->

    <div class="mb-3">

        @Html.LabelFor(
            model => model.Rating,
            new
            {
                @class = "form-label fw-semibold"
            }
        )

        @Html.DropDownListFor(
            model => model.Rating,

            new List<SelectListItem>
            {
                new SelectListItem
                {
                    Value = "0",
                    Text = "-- Select Rating --"
                },

                new SelectListItem
                {
                    Value = "1",
                    Text = "1 - Poor"
                },

                new SelectListItem
                {
                    Value = "2",
                    Text = "2 - Fair"
                },

                new SelectListItem
                {
                    Value = "3",
                    Text = "3 - Good"
                },

                new SelectListItem
                {
                    Value = "4",
                    Text = "4 - Very Good"
                },

                new SelectListItem
                {
                    Value = "5",
                    Text = "5 - Excellent"
                }
            },

            new
            {
                @class = "form-select"
            }
        )

        @Html.ValidationMessageFor(
            model => model.Rating,
            "",
            new
            {
                @class = "text-danger"
            }
        )

    </div>


    <!-- Comments -->

    <div class="mb-3">

        @Html.LabelFor(
            model => model.Comments,
            new
            {
                @class = "form-label fw-semibold"
            }
        )

        @Html.TextAreaFor(
            model => model.Comments,
            new
            {
                @class = "form-control",
                rows = 5,
                placeholder = "Enter your feedback here..."
            }
        )

        @Html.ValidationMessageFor(
            model => model.Comments,
            "",
            new
            {
                @class = "text-danger"
            }
        )

    </div>


    <!-- Submit Button -->

    <div class="d-grid">

        <button type="submit"
                class="btn btn-primary btn-lg">

            Submit Feedback

        </button>

    </div>

</form>
<script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.21.0/jquery.validate.min.js">
</script>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validation-unobtrusive/4.0.0/jquery.validate.unobtrusive.min.js">
</script>
@{
    ViewData["Title"] = "Error";
}

<div class="text-center mt-5">

    <h1 class="text-danger">
        Something went wrong.
    </h1>

    <p>
        An error occurred while processing your request.
    </p>

    <a asp-controller="Feedback"
       asp-action="Index"
       class="btn btn-primary">

        Back to Feedback

    </a>

</div>
html {
    font-size: 14px;
}

body {
    min-height: 100vh;
    background-color: #f5f7fa;
    font-family: Arial, Helvetica, sans-serif;
}

.feedback-card {
    border: none;
    border-radius: 12px;
    overflow: hidden;
}

.card-header {
    padding: 20px;
}

.form-control,
.form-select {
    padding: 11px 14px;
    border-radius: 8px;
}

.form-control:focus,
.form-select:focus {
    box-shadow: 0 0 0 0.2rem rgba(13, 110, 253, 0.15);
}

.btn {
    border-radius: 8px;
}

.text-danger {
    font-size: 0.9rem;
}

.alert {
    border-radius: 8px;
}

footer {
    background-color: #ffffff;
}
document.addEventListener("DOMContentLoaded", function () {

    const form = document.querySelector("form");

    if (form) {

        form.addEventListener("submit", function () {

            const button =
                form.querySelector("button[type='submit']");

            if (button && form.checkValidity()) {

                button.disabled = true;

                button.innerText = "Submitting...";

            }

        });

    }

});
