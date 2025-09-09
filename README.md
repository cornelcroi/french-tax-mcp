[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/cornelcroi-french-tax-mcp-badge.png)](https://mseep.ai/app/cornelcroi-french-tax-mcp)

# French Tax MCP Server

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Python](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)

An MCP (Model Context Protocol) server that provides French individual income tax calculations to AI assistants.

## Current Functionality

This MCP server currently supports:

- **Individual Income Tax Calculations**: Calculate French income tax (impôt sur le revenu) based on net taxable income and household composition (quotient familial)
- **Tax Brackets**: Retrieve current French income tax brackets from official government sources
- **Dynamic Data**: Uses web scraping from service-public.fr to get up-to-date tax information with fallback to hardcoded data

## Data Sources

The current version uses web scraping with MarkItDown to fetch tax information from official French government websites (primarily service-public.fr). In future versions, this may be replaced with official APIs or other more reliable data sources when available.

## Installation

```bash
# Install via pip
pip install french-tax-mcp

# Or install via uv (recommended)
uv pip install french-tax-mcp
```

## MCP Configuration

Add to your MCP configuration file (`~/.config/mcp/mcp.json` or workspace `.kiro/settings/mcp.json`):

```json
{
  "mcpServers": {
    "french-tax-mcp": {
      "command": "uvx",
      "args": ["french-tax-mcp@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## Available Tools

### `calculate_income_tax`
Calculate French individual income tax.

**Parameters:**
- `net_taxable_income`: Net taxable income in euros
- `household_parts`: Number of household parts (quotient familial) - defaults to 1.0
- `year`: Tax year (optional, defaults to current year)

**Example:**
```
Calculate income tax for 50,000€ salary with 2 children
```

### `get_tax_brackets`
Retrieve current French income tax brackets.

**Parameters:**
- `year`: Tax year (optional, defaults to current year)

**Example:**
```
What are the current French tax brackets?
```

## Usage Examples

**Basic calculation:**
```
How much income tax will I pay on 45,000€ salary?
```

**Family situation:**
```
Calculate tax for married couple earning 60,000€ with one child
```

**Tax brackets:**
```
Show me the 2024 French tax brackets
```

## Limitations

- Currently supports only individual income tax calculations for French residents
- Web scraping may occasionally fail (fallback data is used in such cases)
- Tax calculations are for informational purposes only

## TODO - Future Features

The following features may be added in future versions:

- [ ] **LMNP (Location Meublée Non Professionnelle)** - Furnished rental tax calculations
- [ ] **Pinel Investment** - Real estate investment tax benefits
- [ ] **Micro-Enterprise/Auto-Entrepreneur** - Business tax calculations
- [ ] **Corporate Tax** - Company tax calculations
- [ ] **Property Tax** - Real estate tax information
- [ ] **Social Charges** - Social security contributions
- [ ] **Tax Forms** - Detailed form guidance and filling assistance
- [ ] **Official API Integration** - Replace web scraping with official government APIs

## Development

### Local Development

```bash
# Clone the repository
git clone https://github.com/your-username/french-tax-mcp.git
cd french-tax-mcp

# Install in development mode
pip install -e ".[dev]"

# Run the server locally
python -m french_tax_mcp.server --port 8888
```

### Running Tests

```bash
# Run tests
python -m pytest tests/
```

## Legal Notice

This tool provides information for informational purposes only and does not constitute professional tax advice. For advice tailored to your personal situation, please consult a certified public accountant or tax advisor.

Tax information is sourced from official French government websites but may not reflect the most recent changes in tax laws. Always verify calculations with official sources.

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## Links

- **Official French Tax Website**: https://www.impots.gouv.fr
- **MCP Protocol**: https://modelcontextprotocol.io/
