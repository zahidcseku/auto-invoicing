# 🧾 Toggl Track to Rippling Invoice Automation

> One-click invoice generation from your Toggl Track time entries directly to Rippling.

[![Python](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/downloads/)
[![UV](https://img.shields.io/badge/uv-latest-green.svg)](https://github.com/astral-sh/uv)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Demo](#demo)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Development](#development)
- [Testing](#testing)
- [API Documentation](#api-documentation)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [Roadmap](#roadmap)
- [License](#license)
- [Support](#support)

---

## 🎯 Overview

As a freelancer, manually creating invoices from time tracking data is tedious and error-prone. This tool automates the entire workflow:

1. **Fetch** your billable hours from Toggl Track (more time-tracker integration later)
2. **Aggregate** time entries by client and project
3. **Generate** detailed invoice line items
4. **Submit** invoices to Rippling with a single click

Say goodbye to manual data entry and hello to streamlined invoicing! ⚡

---

## ✨ Features

### Core Functionality
- ✅ **One-Click Invoicing** - Generate and submit invoices with a single button press
- ✅ **Smart Aggregation** - Automatically groups time entries by client and project
- ✅ **Billable Hours Only** - Filters to include only billable time entries
- ✅ **Flexible Date Ranges** - Support for "Last Week" or custom date ranges
- ✅ **Detailed Line Items** - Includes project names, hours, rates, and task descriptions

### Technical Features
- 🚀 **Lightning Fast** - Built with UV for blazing-fast dependency management
- 🔒 **Secure** - Environment-based configuration for API credentials
- 📝 **Comprehensive Logging** - Track all operations with Loguru
- 🧪 **Well Tested** - Unit and integration tests with pytest
- 🎨 **Clean Code** - Formatted with Black, linted with Ruff
- 🌐 **Web Interface** - Simple, responsive Flask-based UI

### User Experience
- 💚 **Simple UI** - No technical knowledge required
- 📱 **Responsive Design** - Works on desktop and mobile
- ⏱️ **Real-time Feedback** - Loading indicators and clear success/error messages
- 🔄 **Error Handling** - Graceful error handling with helpful messages

---

### Example Workflow

1. **Select Client**: Choose from your Toggl Track clients
2. **Set Rate**: Enter your hourly rate
3. **Choose Period**: Last week or custom date range
4. **Click Submit**: Invoice is generated and submitted to Rippling
5. **View Result**: See total hours, amount, and invoice ID

---

## 📋 Prerequisites

Before you begin, ensure you have the following:

### Required
- **Python 3.10+** - Download from [python.org](https://www.python.org/downloads/)
- **UV** - Fast Python package installer ([installation guide](https://docs.astral.sh/uv/))
- **Toggl Track Account** - With active time tracking
- **Rippling Account** - With API access enabled

### API Access
- **Toggl Track API Token** - Get from [your profile](https://track.toggl.com/profile)
- **Rippling API Credentials** - Contact Rippling support to enable API access

### Recommended
- **Git** - For version control
- **Make** - For using Makefile commands (optional)
- **VS Code** or **PyCharm** - For development

---

## 🚀 Installation

### Linux/macOS (tested)
```bash
# Clone the repository
git clone https://github.com/zahidcseku/auto-invoicing.git
cd auto-invoicing

# Run setup script
chmod +x setup.sh
./setup.sh

# Edit .env with your API credentials
nano .env  # or use your favorite editor
```

### Other os
Please use your suitable tools to perform the above steps. 


---

## ⚙️ Configuration

### Step 1: Get Your API Credentials

#### Toggl Track API Token
1. Go to [Toggl Track Profile](https://track.toggl.com/profile)
2. Scroll to "API Token" section
3. Click "Click to reveal" to see your token
4. Copy the token

#### Rippling API Access
1. Contact Rippling support or your account manager
2. Request API access for invoice creation
3. They will provide:
   - API Key
   - Company ID
   - API endpoint documentation

> **Note**: Rippling API access varies by subscription plan. Some plans may require an upgrade or additional fees.

### Step 2: Configure Environment Variables

Edit the `.env` file:

```bash
# Toggl Track API Token
TOGGL_API_TOKEN=your_actual_toggl_token_here

# Rippling API Credentials
RIPPLING_API_KEY=your_actual_rippling_key_here
RIPPLING_COMPANY_ID=your_actual_company_id_here
```

> ⚠️ **Security Warning**: Never commit the `.env` file to version control! It's already in `.gitignore`.

### Step 3: Verify Configuration

```bash
# Test that credentials are loaded
uv run python -c "
from dotenv import load_dotenv
import os
load_dotenv()
print('Toggl Token:', '✓' if os.getenv('TOGGL_API_TOKEN') else '✗')
print('Rippling Key:', '✓' if os.getenv('RIPPLING_API_KEY') else '✗')
print('Company ID:', '✓' if os.getenv('RIPPLING_COMPANY_ID') else '✗')
"
```

---

## 🎮 Usage

### Starting the Application

#### Using UV (Recommended)
```bash
uv run python invoice_automation.py
```

#### Using Make
```bash
make run
```

#### Using Flask CLI
```bash
uv run flask run --debug
```

The application will start at: **http://localhost:5000**

### Generating an Invoice

1. **Open your browser** to `http://localhost:5000`

2. **Fill in the form**:
   - **Client Name**: Must match exactly as it appears in Toggl Track
   - **Hourly Rate**: Your billing rate (e.g., `150.00`)
   - **Period**: 
     - "Last Week" - Automatically uses previous Monday-Sunday
     - "Custom Date Range" - Specify exact start and end dates

3. **Click "Generate & Submit Invoice"**

4. **View the result**:
   - Success: Shows total hours, amount, and Rippling invoice ID
   - Error: Displays helpful error message with troubleshooting steps

### Example: Generating Last Week's Invoice

```
Client Name: Acme Corporation
Hourly Rate: 150.00
Period: Last Week

Result:
✅ Invoice Created Successfully!
Client: Acme Corporation
Period: 2025-10-13 to 2025-10-19
Total Hours: 42.5
Total Amount: $6,375.00
Invoice ID: INV-2025-001
```

### Programmatic Usage

You can also use the tool programmatically:

```python
from invoice_automation import InvoiceGenerator
import os

generator = InvoiceGenerator(
    toggl_token=os.getenv("TOGGL_API_TOKEN"),
    rippling_key=os.getenv("RIPPLING_API_KEY"),
    rippling_company_id=os.getenv("RIPPLING_COMPANY_ID")
)

# Generate invoice for last week
result = generator.generate_invoice(
    client_name="Acme Corporation",
    hourly_rate=150.00
)

print(f"Invoice created: ${result['total_amount']}")

# Or with custom date range
result = generator.generate_invoice(
    client_name="TechCorp",
    hourly_rate=200.00,
    start_date="2025-10-01",
    end_date="2025-10-31"
)
```

---

## 📁 Project Structure

```
toggl-rippling-invoicing/
├── .venv/                      # Virtual environment (auto-created by UV)
├── .github/                    # GitHub Actions workflows
│   └── workflows/
│       └── ci.yml
├── docs/                       # Documentation
│   ├── API_SETUP.md
│   ├── DEPLOYMENT.md
│   └── TROUBLESHOOTING.md
├── src/                        # Source code (optional organization)
│   ├── __init__.py
│   ├── toggl_client.py        # Toggl API integration
│   ├── rippling_client.py     # Rippling API integration
│   └── invoice_generator.py  # Core business logic
├── tests/                      # Test suite
│   ├── __init__.py
│   ├── test_toggl_client.py
│   ├── test_rippling_client.py
│   └── test_invoice_generator.py
├── .env                        # Environment variables (not in git)
├── .env.example               # Environment template
├── .gitignore                 # Git ignore rules
├── .python-version            # Python version pin (3.11)
├── invoice_automation.py      # Main application
├── Makefile                   # Development commands
├── pyproject.toml            # Project configuration
├── uv.lock                    # Dependency lockfile
├── README.md                  # This file
├── setup.sh                   # Setup script (Linux/Mac)
└── setup.ps1                  # Setup script (Windows)
```

---

## 🛠️ Development

### Setting Up Development Environment

```bash
# Install with dev dependencies
uv sync --extra dev

# Verify installation
uv run python --version
uv run pytest --version
```

### Running Tests

```bash
# Run all tests
uv run pytest

# Run with coverage
uv run pytest --cov

# Run specific test file
uv run pytest tests/test_toggl_client.py

# Run with verbose output
uv run pytest -v
```

Or using Make:
```bash
make test              # Run all tests
make test-coverage     # Run with coverage report
```

### Code Quality

#### Format Code
```bash
# Format with Black
uv run black .

# Or use Make
make format
```

#### Lint Code
```bash
# Lint with Ruff
uv run ruff check .

# Auto-fix issues
uv run ruff check --fix .

# Or use Make
make lint
make lint-fix
```

#### Type Checking
```bash
# Type check with MyPy
uv run mypy .

# Or use Make
make type-check
```

#### Run All Checks
```bash
# Format, lint, type-check, and test
make all-checks
```

### Adding Dependencies

```bash
# Add production dependency
uv add requests

# Add development dependency
uv add --dev pytest-mock

# Or using Make
make deps-add PACKAGE=requests
make deps-add-dev PACKAGE=pytest-mock
```

### Git Workflow

```bash
# Create feature branch
git checkout -b feature/TRIA-XX-feature-name

# Make changes and commit
git add .
git commit -m "TRIA-XX: Add feature description"

# Run pre-commit checks
make pre-commit

# Push to remote
git push origin feature/TRIA-XX-feature-name

# Create pull request on GitHub
```

---

## 🧪 Testing

### Test Structure

```
tests/
├── test_toggl_client.py       # Toggl API integration tests
├── test_rippling_client.py    # Rippling API integration tests
├── test_invoice_generator.py # Business logic tests
└── conftest.py                # Pytest fixtures
```

### Writing Tests

Example test:

```python
import pytest
from invoice_automation import TogglClient

def test_get_time_entries():
    client = TogglClient("test_token")
    entries = client.get_time_entries("2025-10-13", "2025-10-19")
    
    assert len(entries) > 0
    assert entries[0].billable is True
    assert entries[0].hours > 0
```

### Coverage Report

```bash
# Generate HTML coverage report
uv run pytest --cov --cov-report=html

# View report
open htmlcov/index.html  # macOS
xdg-open htmlcov/index.html  # Linux
start htmlcov/index.html  # Windows
```

---

## 📚 API Documentation

### Toggl Track API

**Base URL**: `https://api.track.toggl.com/api/v9`

**Authentication**: Basic Auth with API token

**Key Endpoints**:
- `GET /me` - Get user info and workspace
- `GET /me/time_entries` - Get time entries

**Documentation**: [Toggl Track API Docs](https://developers.track.toggl.com/)

### Rippling API

**Base URL**: `https://api.rippling.com/platform/api`

**Authentication**: Bearer token

**Key Endpoints**:
- `POST /invoices` - Create invoice (endpoint may vary)

**Documentation**: Contact Rippling support for API documentation

> **Note**: Rippling's exact API endpoints depend on your subscription plan. Consult with Rippling support for accurate endpoint information.

---

## 🐛 Troubleshooting

### Common Issues

#### Issue: "No billable hours found for client"

**Causes**:
- Client name doesn't match exactly (case-sensitive)
- No time entries marked as billable in Toggl
- Wrong date range selected

**Solution**:
```bash
# Check client names in Toggl
uv run python -c "
from invoice_automation import TogglClient
import os
client = TogglClient(os.getenv('TOGGL_API_TOKEN'))
entries = client.get_time_entries('2025-10-13', '2025-10-19')
clients = set(e.client for e in entries)
print('Available clients:', clients)
"
```

#### Issue: "Failed to create invoice in Rippling"

**Causes**:
- Invalid API credentials
- Rippling API endpoint incorrect
- API access not enabled

**Solution**:
1. Verify credentials in `.env`
2. Contact Rippling support to confirm API access
3. Check `invoice_automation.log` for detailed error

#### Issue: "Authentication failed"

**Solution**:
```bash
# Test Toggl connection
uv run python -c "
from invoice_automation import TogglClient
import os
client = TogglClient(os.getenv('TOGGL_API_TOKEN'))
workspace_id = client.get_workspace_id()
print(f'✅ Connected to Toggl! Workspace ID: {workspace_id}')
"
```

#### Issue: UV commands not found

**Solution**:
```bash
# Reinstall UV
curl -LsSf https://astral.sh/uv/install.sh | sh

# Add to PATH
export PATH="$HOME/.cargo/bin:$PATH"

# Reload shell
source ~/.bashrc  # or ~/.zshrc
```

### Debug Mode

Enable debug logging:

```python
# In invoice_automation.py
logger.add("debug.log", level="DEBUG")
```

View logs:
```bash
tail -f invoice_automation.log
```

### Getting Help

1. Check the [Troubleshooting Guide](docs/TROUBLESHOOTING.md)
2. Search [existing issues](https://github.com/yourusername/toggl-rippling-invoicing/issues)
3. Create a [new issue](https://github.com/yourusername/toggl-rippling-invoicing/issues/new)
4. Contact support: your.email@example.com

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

### Reporting Bugs

1. Check if the bug is already reported in [Issues](https://github.com/yourusername/toggl-rippling-invoicing/issues)
2. Create a new issue with:
   - Clear title and description
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment details (OS, Python version, etc.)
   - Relevant logs (remove sensitive data!)

### Suggesting Features

1. Check [existing feature requests](https://github.com/yourusername/toggl-rippling-invoicing/issues?q=is%3Aissue+label%3Aenhancement)
2. Create a new issue with label `enhancement`
3. Describe the feature and use case
4. Explain why it would be useful

### Pull Requests

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/TRIA-XX-description`
3. Make your changes
4. Run tests: `make all-checks`
5. Commit with descriptive message: `git commit -m "TRIA-XX: Add feature"`
6. Push to your fork: `git push origin feature/TRIA-XX-description`
7. Create a Pull Request

### Development Guidelines

- Follow existing code style (Black + Ruff)
- Add tests for new features
- Update documentation
- Keep commits atomic and well-described
- Reference Jira issues in commits (TRIA-XX)

---

## 🗺️ Roadmap

See [ROADMAP.md](docs/ROADMAP.md) for detailed plans.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](https://mit-license.org) file for details.

---

## 💬 Support

### Documentation
- [Installation Guide](docs/INSTALLATION.md)
- [API Setup Guide](docs/API_SETUP.md)
- [Deployment Guide](docs/DEPLOYMENT.md)
- [Troubleshooting](docs/TROUBLESHOOTING.md)

### Community
- [GitHub Discussions](https://github.com/yourusername/toggl-rippling-invoicing/discussions)
- [Issue Tracker](https://github.com/zahidcseku/auto-invoicing/issues)

### Contact
- **Email**: zahidcseku@gmail.com.com

---

## 🙏 Acknowledgments

- [Toggl Track](https://toggl.com/track/) - Excellent time tracking platform
- [Rippling](https://www.rippling.com/) - Comprehensive HR and payroll solution
- [UV](https://github.com/astral-sh/uv) - Lightning-fast Python package installer
- [Flask](https://flask.palletsprojects.com/) - Lightweight web framework
- [Loguru](https://github.com/Delgan/loguru) - Python logging made simple

---


## 📊 Project Stats

![GitHub stars](https://img.shields.io/github/stars/zahidcseku/auto-invoicing?style=social)
![GitHub forks](https://img.shields.io/github/forks/zahidcseku/auto-invoicing?style=social)
![GitHub watchers](https://img.shields.io/github/watchers/zahidcseku/auto-invoicing?style=social)
![GitHub last commit](https://img.shields.io/github/last-commit/zahidcseku/auto-invoicing?style=social)
![GitHub issues](https://img.shields.io/github/issues/zahidcseku/auto-invoicing?style=social)
![GitHub pull requests](https://img.shields.io/github/issues-pr/zahidcseku/auto-invoicing?style=social)

---

<p align="center">
  Made with ❤️ by <a href="https://github.com/zahidcseku">Md Zahidul Islam</a>
</p>

<p align="center">
  <a href="#-table-of-contents">Back to Top ⬆️</a>
</p>