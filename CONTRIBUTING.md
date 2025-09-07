# Contributing to Weather Forecast Dashboard

Thank you for your interest in contributing to the Weather Forecast Dashboard! This document provides guidelines for contributing to this project.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Development Guidelines](#development-guidelines)
- [Pull Request Process](#pull-request-process)
- [Issue Reporting](#issue-reporting)
- [Community](#community)

## 📜 Code of Conduct

### Our Pledge

We are committed to providing a friendly, safe, and welcoming environment for all contributors, regardless of experience level, gender identity, sexual orientation, disability, personal appearance, body size, race, ethnicity, age, religion, or nationality.

### Expected Behavior

- **Be respectful** and inclusive in your communication
- **Be collaborative** and help others learn
- **Be constructive** when giving feedback
- **Be patient** with newcomers and questions
- **Respect different viewpoints** and experiences

### Unacceptable Behavior

- Harassment, trolling, or discriminatory language
- Personal attacks or political discussions
- Publishing private information without permission
- Spam or excessive self-promotion

## 🚀 Getting Started

### Prerequisites

Before contributing, ensure you have:

- **Microsoft PowerBI Desktop** (latest version)
- **Git** for version control
- **Weather API access** (for testing)
- Basic knowledge of:
  - PowerBI development
  - DAX (Data Analysis Expressions)
  - M Language (Power Query)
  - REST APIs

### Development Setup

1. **Fork the repository**
   ```bash
   # Click "Fork" on GitHub, then clone your fork
   git clone https://github.com/YOUR_USERNAME/Weather-Forecast-Dashboard.git
   cd Weather-Forecast-Dashboard
   ```

2. **Set up upstream remote**
   ```bash
   git remote add upstream https://github.com/ORIGINAL_OWNER/Weather-Forecast-Dashboard.git
   ```

3. **Install PowerBI Desktop**
   - Download from [Microsoft PowerBI](https://powerbi.microsoft.com/desktop/)
   - Install the latest version

4. **Configure API Keys**
   - Obtain API keys from weather providers
   - Set up local configuration (see [`docs/API_SETUP.md`](docs/API_SETUP.md))

5. **Test the setup**
   - Open `src/Weather_forecast_Dashboard.pbix`
   - Verify data connections work
   - Test all visualizations

## 🤝 How to Contribute

### Types of Contributions

We welcome various types of contributions:

1. **🐛 Bug Reports**
   - Report issues with dashboard functionality
   - Document unexpected behavior
   - Provide reproduction steps

2. **✨ Feature Requests**
   - Suggest new weather parameters
   - Propose UI/UX improvements
   - Request additional data sources

3. **📖 Documentation**
   - Improve setup instructions
   - Add usage examples
   - Create video tutorials

4. **🔧 Code Contributions**
   - Fix bugs in DAX formulas
   - Optimize data model performance
   - Add new visualizations

5. **🎨 Design Improvements**
   - Create new themes/backgrounds
   - Design weather icons
   - Improve dashboard layout

### Contribution Workflow

1. **Check existing issues** to avoid duplicates
2. **Create an issue** to discuss your idea (for major changes)
3. **Fork and create a branch** for your work
4. **Make your changes** following our guidelines
5. **Test thoroughly** before submitting
6. **Create a pull request** with detailed description

## 🛠️ Development Guidelines

### PowerBI Development Standards

#### 1. File Organization
```
src/
├── Weather_forecast_Dashboard.pbix    # Main dashboard file
├── data_models/                       # Data model documentation
├── dax_measures/                      # Custom DAX formulas
└── query_templates/                   # M language queries
```

#### 2. Naming Conventions

**Tables:**
- Use PascalCase: `WeatherCurrent`, `LocationData`
- Descriptive names: `ForecastDaily` not `Table1`

**Measures:**
- Use descriptive names: `AverageTemperature`, `WindSpeedKPH`
- Group related measures: `Temp - Current`, `Temp - Average`

**Columns:**
- Use PascalCase: `LocationID`, `LastUpdate`
- Avoid abbreviations: `Temperature` not `Temp`

#### 3. DAX Best Practices

```dax
// ✅ Good: Clear, readable DAX
AverageTemperature = 
CALCULATE(
    AVERAGE(WeatherData[Temperature]),
    REMOVEFILTERS(WeatherData[Date])
)

// ❌ Avoid: Complex, unreadable formulas
BadFormula = CALCULATE(AVERAGE(WeatherData[Temperature]),REMOVEFILTERS(WeatherData[Date]),FILTER(WeatherData,WeatherData[Location]="NYC"))
```

#### 4. Performance Guidelines

- **Use variables** to avoid recalculating expressions
- **Filter early** in data transformation
- **Create aggregation tables** for large datasets
- **Optimize relationships** and cardinality

### Code Quality Standards

#### Comments and Documentation
```dax
// Purpose: Calculate temperature trend indicator
// Returns: "↑" for increasing, "↓" for decreasing, "→" for stable
TemperatureTrend = 
VAR CurrentTemp = SELECTEDVALUE(WeatherCurrent[Temperature])
VAR PreviousTemp = CALCULATE(
    AVERAGE(WeatherCurrent[Temperature]),
    DATEADD('Calendar'[Date], -1, DAY)
)
VAR TempDifference = CurrentTemp - PreviousTemp
RETURN
    SWITCH(
        TRUE(),
        TempDifference > 1, "↑",
        TempDifference < -1, "↓",
        "→"
    )
```

#### Testing Requirements
- **Test all measures** with sample data
- **Verify calculations** manually for accuracy
- **Check performance** with large datasets
- **Validate data refresh** functionality

### Visual Design Guidelines

#### Color Scheme
- **Primary:** Blues and whites for sky/cloud themes
- **Temperature:** Red-orange for hot, blue for cold
- **Alerts:** Yellow for warnings, red for dangers
- **Neutral:** Grays for secondary information

#### Typography
- **Headers:** Bold, clear fonts
- **Data:** Monospace for numbers
- **Labels:** Sans-serif for readability

#### Layout Principles
- **Consistency:** Align elements properly
- **Hierarchy:** Important data more prominent
- **White Space:** Don't overcrowd visuals
- **Accessibility:** High contrast, readable fonts

## 📬 Pull Request Process

### Before Submitting

1. **Sync with upstream**
   ```bash
   git fetch upstream
   git checkout main
   git merge upstream/main
   ```

2. **Create feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**
   - Follow coding standards
   - Add appropriate comments
   - Test thoroughly

4. **Commit changes**
   ```bash
   git add .
   git commit -m "feat: add temperature trend visualization
   
   - Added DAX measure for temperature trends
   - Created visual indicator for temp changes
   - Updated documentation"
   ```

### Pull Request Guidelines

#### Title Format
- **feat:** New feature addition
- **fix:** Bug fix
- **docs:** Documentation changes
- **style:** Visual/UI improvements
- **perf:** Performance improvements
- **test:** Testing additions

#### Description Template
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Performance improvement

## Testing
- [ ] Tested with sample data
- [ ] Verified calculations
- [ ] Checked performance impact
- [ ] Validated on different screen sizes

## Screenshots
(If applicable, add screenshots of changes)

## Checklist
- [ ] Code follows project standards
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] Changes tested thoroughly
```

### Review Process

1. **Automated checks** will run on your PR
2. **Maintainer review** will occur within 48 hours
3. **Address feedback** promptly and professionally
4. **Approved PRs** will be merged by maintainers

## 🐛 Issue Reporting

### Bug Reports

Use this template for bug reports:

```markdown
**Bug Description**
Clear description of the issue

**Steps to Reproduce**
1. Step one
2. Step two
3. Step three

**Expected Behavior**
What should happen

**Actual Behavior**
What actually happens

**Environment**
- PowerBI Desktop version:
- Operating System:
- Browser (if applicable):

**Screenshots**
(If applicable)

**Additional Context**
Any other relevant information
```

### Feature Requests

Use this template for feature requests:

```markdown
**Feature Summary**
Brief description of the feature

**Problem Statement**
What problem does this solve?

**Proposed Solution**
Detailed description of your idea

**Alternatives Considered**
Other solutions you've thought about

**Additional Context**
Mockups, examples, or related issues
```

## 💬 Community

### Getting Help

- **Documentation:** Check [`docs/`](docs/) folder first
- **Issues:** Search existing issues before creating new ones
- **Discussions:** Use GitHub Discussions for questions
- **Email:** Contact maintainers for private matters

### Communication Channels

- **GitHub Issues:** Bug reports and feature requests
- **GitHub Discussions:** General questions and ideas
- **Pull Requests:** Code review and collaboration

### Recognition

Contributors will be:
- **Listed** in project acknowledgments
- **Credited** in release notes
- **Invited** to become maintainers (for significant contributions)

## 📚 Resources

### Learning Materials
- [PowerBI Documentation](https://docs.microsoft.com/en-us/power-bi/)
- [DAX Guide](https://dax.guide/)
- [Power Query M Reference](https://docs.microsoft.com/en-us/powerquery-m/)

### Tools
- [DAX Formatter](https://www.daxformatter.com/)
- [PowerBI Visuals Gallery](https://appsource.microsoft.com/en-us/marketplace/apps?product=power-bi-visuals)
- [Git Best Practices](https://git-scm.com/book)

---

## 🙏 Thank You

Your contributions make this project better for everyone. Whether you're fixing a small typo or adding a major feature, every contribution is valued and appreciated!

---

*For questions about contributing, please create an issue or contact the maintainers.*