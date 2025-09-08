[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/cornelcroi-french-tax-mcp-badge.png)](https://mseep.ai/app/cornelcroi-french-tax-mcp)

# French Tax MCP Server

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Python](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![PyPI](https://img.shields.io/pypi/v/french-tax-mcp)](https://pypi.org/project/french-tax-mcp/)

An MCP (Model Context Protocol) server that provides French tax calculations and information to AI assistants. Get accurate tax calculations, understand French tax schemes, and access tax form guidance through your AI assistant.

## 🎯 What This Solves

French tax calculations are complex and error-prone. This MCP server enables AI assistants to:

- **Calculate taxes accurately** for individuals and businesses
- **Explain French tax schemes** like Pinel, LMNP, micro-enterprise
- **Provide tax form guidance** with specific instructions
- **Access current tax information** from official sources when available

Perfect for French residents, tax professionals, and anyone dealing with French taxation.

## ✨ Features

### 🧮 **Tax Calculations**

- **Income Tax**: Calculate French income tax with progressive brackets and quotient familial
- **Pinel Investment**: Calculate tax benefits for Pinel real estate investments
- **LMNP/LMP**: Furnished rental tax calculations (micro and real regimes)
- **Micro-Enterprise**: Tax calculations for auto-entrepreneurs and micro-enterprises
- **Household Parts**: Automatic quotient familial calculation based on family situation

### 📋 **Tax Information**

- **Tax Schemes**: Detailed information on Pinel, LMNP, LMP, and other French tax schemes
- **Tax Forms**: Guidance on forms 2042, 2044, 2031, and other tax declarations
- **Tax Brackets**: Current and historical French income tax brackets
- **Procedures**: Step-by-step tax filing procedures and deadlines

### 🔄 **Data Sources**

- **Built-in Data**: Comprehensive tax brackets, rates, and scheme details for 2023-2025
- **Web Scraping**: Attempts to fetch current information from official sites when possible
- **Fallback System**: Reliable fallback to built-in data when scraping fails
- **Smart Caching**: Reduces load on government websites

## 🚀 Quick Start

### Installation

```bash
# Install via pip
pip install french-tax-mcp

# Or install via uv (recommended)
uv pip install french-tax-mcp
```

### MCP Configuration

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

### Docker Usage

```bash
# Build the image
docker build -t french-tax-mcp .

# Run the container
docker run -p 8888:8888 french-tax-mcp
```

## 🎯 Key Use Cases

**For Individuals:**

- "How much income tax will I pay on 50,000€ salary with 2 children?"
- "Should I invest in a Pinel property for 300,000€?"
- "What are the tax benefits of LMNP furnished rental?"

**For Entrepreneurs:**

- "Calculate my micro-enterprise taxes for 40,000€ services revenue"
- "What's the difference between micro-enterprise and auto-entrepreneur?"
- "How much will I save with ACCRE in my first year?"

**For Tax Professionals:**

- Quick access to current tax brackets and rates
- Detailed tax scheme calculations and explanations
- Form guidance and filing procedures

## 🛠️ MCP Tools

### 1. **get_tax_brackets**

Retrieves current French income tax brackets.

**Parameters:**

- `year` (optional): Tax year (defaults to current year)

**Example Usage:**

```
What are the current French tax brackets?
```

**Example Output:**

```json
{
  "status": "success",
  "data": {
    "year": 2024,
    "brackets": [
      { "min": 0, "max": 11294, "rate": 0 },
      { "min": 11295, "max": 28797, "rate": 11 },
      { "min": 28798, "max": 82341, "rate": 30 },
      { "min": 82342, "max": 177106, "rate": 41 },
      { "min": 177107, "max": null, "rate": 45 }
    ]
  }
}
```

### 2. **calculate_income_tax**

Calculates French income tax based on income and household composition.

**Parameters:**

- `net_taxable_income`: Net taxable income in euros
- `household_parts`: Number of household parts (quotient familial)
- `year` (optional): Tax year

**Example Usage:**

```
Calculate income tax for 50,000€ salary with 2 children
```

**Example Output:**

```json
{
  "status": "success",
  "data": {
    "net_taxable_income": 50000,
    "household_parts": 2.0,
    "total_tax": 3847.5,
    "average_tax_rate": 7.7,
    "marginal_tax_rate": 30
  }
}
```

### 3. **calculate_pinel_benefit**

Calculates Pinel real estate investment tax benefits.

**Parameters:**

- `property_price`: Property price in euros
- `commitment_period`: Commitment period (6, 9, or 12 years)
- `acquisition_date`: Acquisition date (YYYY-MM-DD)

**Example Usage:**

```
Calculate Pinel benefit for 300,000€ property with 9-year commitment
```

**Example Output:**

```json
{
  "status": "success",
  "data": {
    "property_price": 300000,
    "commitment_period": 9,
    "total_reduction": 45000,
    "annual_reduction": 5000,
    "rate": 15.0
  }
}
```

### 4. **calculate_lmnp_benefit**

Calculates LMNP (furnished rental) tax benefits.

**Parameters:**

- `annual_rent`: Annual rental income in euros
- `expenses`: Annual expenses (for 'reel' regime)
- `property_value`: Property value (for 'reel' regime)
- `furniture_value`: Furniture value (for 'reel' regime)
- `regime`: Tax regime ('micro' or 'reel')

**Example Usage:**

```
Calculate LMNP benefit for 24,000€ annual rent in micro regime
```

### 5. **calculate_micro_enterprise_tax**

Calculates taxes for micro-enterprise regime.

**Parameters:**

- `annual_revenue`: Annual revenue in euros
- `activity_type`: Activity type ('commercial', 'services', 'liberal')
- `accre_eligible`: ACCRE eligibility (boolean)
- `year` (optional): Tax year

**Example Usage:**

```
Calculate micro-enterprise tax for 40,000€ services revenue
```

### 6. **get_scheme_details**

Retrieves detailed information about tax schemes.

**Parameters:**

- `scheme_name`: Scheme name ('pinel', 'lmnp', 'lmp')
- `year` (optional): Tax year

**Example Usage:**

```
Tell me about the Pinel tax scheme
```

### 7. **get_form_details**

Provides information about tax forms.

**Parameters:**

- `form_number`: Form number ('2042', '2044', '2031', etc.)
- `year` (optional): Tax year

**Example Usage:**

```
What is form 2044 used for?
```

### 8. **get_tax_procedure**

Retrieves tax procedure information from service-public.fr.

**Parameters:**

- `procedure_name`: Procedure name ('declaration_revenus', 'credit_impot', etc.)

**Example Usage:**

```
How do I declare my income in France?
```

### 9. **get_tax_deadlines**

Gets tax deadlines and important dates.

**Parameters:**

- `year` (optional): Tax year

**Example Usage:**

```
What are the tax deadlines for 2024?
```

### 10. **get_tax_article**

Retrieves tax law articles from legifrance.gouv.fr.

**Parameters:**

- `article_id`: Article identifier

**Example Usage:**

```
Show me article 200 of the tax code
```

### 11. **search_tax_law**

Searches tax law articles.

**Parameters:**

- `query`: Search query

**Example Usage:**

```
Search for tax laws about real estate investment
```

### 12. **generate_tax_report**

Generates comprehensive tax reports.

**Parameters:**

- `tax_data`: Tax information data
- `topic_name`: Report topic name
- `output_file` (optional): File path to save report
- `format`: Output format ('markdown' or 'csv')

## 📝 Usage Examples

### English Examples

**Question:** "How much income tax would I pay on a 60,000€ salary if I'm married with one child?"

**AI Response:** Based on the French tax calculation:

- Net taxable income: €60,000
- Household parts: 2.5 (married couple + 0.5 for one child)
- Income per part: €24,000
- Total income tax: €1,287
- Average tax rate: 2.15%
- Marginal tax rate: 11%

**Question:** "Is a Pinel investment worth it for a €250,000 property?"

**AI Response:** For a €250,000 Pinel investment with 9-year commitment:

- Total tax reduction: €37,500 (15% of investment)
- Annual tax reduction: €4,167
- This represents significant tax savings, but consider rental yield and property appreciation potential.

### Exemples en Français

**Question:** "Combien d'impôts vais-je payer sur 45 000€ de revenus en micro-entreprise de services ?"

**Réponse IA:** Pour une micro-entreprise de services avec 45 000€ de CA :

- **Chiffre d'affaires** : 45 000€
- **Abattement forfaitaire** : 50% (pour les services)
- **Revenu imposable** : 22 500€
- **Cotisations sociales** : 9 900€ (22% du CA)
- **Impôt sur le revenu** : 3 375€ (15% du CA)
- **Total des prélèvements** : 13 275€
- **Revenu net** : 31 725€

**Question:** "Quel est l'impôt sur le revenu pour un salaire de 50 000€ avec 2 enfants ?"

**Réponse IA:** Calcul de l'impôt sur le revenu 2024 :

- **Revenu net imposable** : 50 000€
- **Parts fiscales** : 2,0 (vous + 0,5 par enfant × 2)
- **Revenu par part** : 25 000€
- **Impôt total** : 3 015€
- **Taux moyen** : 6,03%
- **Taux marginal** : 11%

Vous conserverez environ **46 985€** après impôt sur le revenu.

**Question:** "Quels sont les avantages fiscaux d'un investissement Pinel de 300 000€ ?"

**Réponse IA:** Pour un investissement Pinel de 300 000€ avec engagement 9 ans :

- **Réduction d'impôt totale** : 45 000€ (15% de l'investissement)
- **Réduction annuelle** : 5 000€ pendant 9 ans
- **Économie d'impôt** : Jusqu'à 15% du prix d'achat
- **Conditions** : Location pendant minimum 6 ans, plafonds de loyers respectés

**Question:** "Comment calculer les impôts en LMNP pour 24 000€ de loyers annuels ?"

**Réponse IA:** En régime micro-BIC (LMNP) :

- **Loyers annuels** : 24 000€
- **Abattement forfaitaire** : 50% (12 000€)
- **Revenu imposable** : 12 000€
- **Pas de charges sociales** sur les revenus locatifs
- **Imposition** : Au barème progressif de l'impôt sur le revenu

En régime réel, vous pourriez déduire les charges réelles et amortir le bien.

**Question:** "Quelles sont les tranches d'imposition 2024 ?"

**Réponse IA:** Barème de l'impôt sur le revenu 2024 :

- **0€ à 11 294€** : 0%
- **11 295€ à 28 797€** : 11%
- **28 798€ à 82 341€** : 30%
- **82 342€ à 177 106€** : 41%
- **Au-delà de 177 106€** : 45%

Ces tranches s'appliquent au revenu par part fiscale (quotient familial).

**Question:** "Quelle différence entre micro-entreprise et auto-entrepreneur ?"

**Réponse IA:** Depuis 2016, **micro-entreprise** et **auto-entrepreneur** désignent le même régime :

- **Plafonds 2024** :
  - Vente : 188 700€
  - Services : 77 700€
- **Abattements forfaitaires** :
  - Vente : 71%
  - Services BIC : 50%
  - Services BNC : 34%
- **Cotisations sociales** : 12,3% à 21,2% selon l'activité
- **TVA** : Franchise jusqu'aux seuils (85 800€ vente, 34 400€ services)

**Question:** "Comment déclarer mes revenus de location meublée ?"

**Réponse IA:** Déclaration LMNP :

- **Formulaire** : 2042-C-PRO (micro-BIC) ou 2031 (régime réel)
- **Case micro-BIC** : 5ND (vous) ou 5OD (conjoint)
- **Montant à déclarer** : Loyers bruts perçus
- **Abattement** : 50% appliqué automatiquement en micro-BIC
- **Seuil** : Passage au réel obligatoire si > 72 600€

**Question:** "Puis-je bénéficier de l'ACCRE en micro-entreprise ?"

**Réponse IA:** L'ACCRE (maintenant ACRE) permet :

- **Réduction de cotisations** : 50% la première année
- **Conditions** : Demandeur d'emploi, bénéficiaire RSA, jeune de moins de 26 ans, etc.
- **Durée** : 12 mois à partir du début d'activité
- **Demande** : À faire dans les 45 jours suivant la création
- **Économie** : Environ 1 500€ à 3 000€ selon le chiffre d'affaires

## 🏗️ Development

### Project Structure

```
french-tax-mcp/
├── french_tax_mcp/
│   ├── __init__.py
│   ├── server.py              # Main MCP server
│   ├── constants.py           # All constants and configuration
│   ├── scrapers/
│   │   ├── __init__.py
│   │   ├── base_scraper.py    # Base scraper with common functionality
│   │   ├── impots_scraper.py  # impots.gouv.fr scraper
│   │   ├── service_public_scraper.py  # service-public.fr scraper
│   │   └── legal_scraper.py   # legifrance.gouv.fr scraper
│   ├── analyzers/
│   │   ├── __init__.py
│   │   ├── income_analyzer.py    # Income tax calculations
│   │   ├── business_analyzer.py  # Business tax calculations
│   │   └── property_analyzer.py  # Property tax calculations
│   ├── static/templates/
│   │   └── report_template.py # Report templates
│   └── report_generator.py    # Report generation
├── tests/                     # All test files
├── scripts/                   # Development scripts
├── .github/workflows/         # GitHub Actions
├── README.md
├── pyproject.toml
├── LICENSE
└── Dockerfile
```

### Running Tests

```bash
# Install development dependencies
pip install -e ".[dev]"

# Run tests
pytest tests/

# Run specific test
python tests/test_tax_brackets.py
```

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

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

### Areas for Contribution

- **New Tax Schemes**: Add support for additional French tax schemes
- **Enhanced Scrapers**: Improve web scraping reliability and coverage
- **Calculations**: Add more tax calculation scenarios
- **Documentation**: Improve examples and documentation
- **Testing**: Add more comprehensive tests

## 📄 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## ⚖️ Legal Notice

This tool provides information for informational purposes only and does not constitute professional tax advice. For advice tailored to your personal situation, please consult a certified public accountant or tax advisor. Information is extracted from official government websites but may not reflect the most recent changes in tax laws.

## 🔗 Links

- **PyPI Package**: https://pypi.org/project/french-tax-mcp/
- **GitHub Repository**: https://github.com/your-username/french-tax-mcp
- **Official French Tax Website**: https://www.impots.gouv.fr
- **MCP Protocol**: https://modelcontextprotocol.io/

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/your-username/french-tax-mcp/issues)
- **Discussions**: [GitHub Discussions](https://github.com/your-username/french-tax-mcp/discussions)

---

Made with ❤️ for the French tax community
