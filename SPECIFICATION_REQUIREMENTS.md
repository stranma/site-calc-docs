# Site Energy System Optimization Platform - Specification Requirements

## Document Purpose

This document captures the complete requirements and context for creating a client-facing specification document for the Site Energy System Optimization Platform (site-calc2). This serves as a reference for future work on the specification.

## Project Context

### Software Background

The project is an energy system optimization software that optimizes distributed energy systems for commercial and industrial facilities.

**Key Quote from Client:**
> "The client is big consultation company which would like to use this software for their big clients. These clients are interested mainly in what optimal site should look like (e.g. battery size and power) and they need some estimates for their bank loans applications."

### Primary Use Case

**Main Application:** Sizing battery with renewables is the most common use case.

**Critical Distinction - What to Document vs Current State:**
> "Important thing is that the documentation should be tailored to what it should do, not strictly what it is now. So e.g. if CHP power is subject of optimization given cost of 0,5MW in some facility, this is something we can deliver very easily but it is not implemented now."

This means the specification should describe **capabilities we can deliver**, not just what exists today.

### Client and End User Profile

**Primary Client:**
- Large consultation company
- Serves big commercial/industrial clients
- Needs to support bank loan applications for their clients

**End Use:**
- Economic output to compute IRR, ROI, etc.
- Ideal parameters of site (optimal sizing)
- Time series optimization is **not very important** - flows are less critical
- Main focus: Economic results and optimal sizing parameters

**Typical Workflow:**
> "The simulation will be probably run multiple times given monte carlo market prices for next 20 years etc."

**Important Note:** Monte Carlo is NOT part of this software - it's just the optimization in a bigger loop.

### Feature Scope

**Must Include in Documentation:**

1. **Device Modeling:**
   - Battery (size and power optimization)
   - Photovoltaics (PV capacity)
   - Combined Heat and Power (CHP)
   - Heat Accumulator

2. **Multi-Material Flows:**
   - Electricity
   - Heat
   - Gas

3. **Optimization Capabilities:**
   - Device sizing optimization (capacities, powers as decision variables, not just fixed inputs)
   - Operational optimization (hourly dispatch)
   - Economic objective functions

4. **Economic Model:**
   - **CAPEX** (Capital Expenditure)
   - **OPEX** (Operating Expenditure)
   - **Revenue Streams:**
     - Energy sales
     - Grid services (aFRR - automatic Frequency Restoration Reserve)
     - Avoided costs
   - **Financial Metrics:**
     - NPV (Net Present Value)
     - IRR (Internal Rate of Return)
     - ROI (Return on Investment)
     - Payback period

5. **Grid Services:**
   - aFRR (automatic Frequency Restoration Reserve) is confirmed for inclusion
   - Peak shaving as revenue stream

6. **Uncertainty Handling:**
   - Document that the system supports Monte Carlo scenarios (external)
   - Price scenarios for 20-year horizon
   - Degradation modeling
   - Multiple scenario comparison

**Time Series Consideration:**
> "Time series optimization is not very important, because for this client flows are not so important (only maybe to illustrate typical flows in some interesting dates)."

This means: time series results are secondary - shown mainly for illustration of typical days, not the primary deliverable.

### Geographic Scope

**Primary Market:** Czech Republic (CZ)

**Czech-Specific Requirements:**
> "We are mainly interested in CZ now. You can add CZ specifics to document, like what unit fixed cost should be included in Electricity input max size."

**CZ Market Specifics to Document:**
- Distribution tariffs structure
- Reserved capacity fees (CZK/kW/month) - typically 200-400 CZK/kW/month for commercial
- Time-of-use tariffs
- Network charges
- aFRR market rules and requirements
- Capacity and energy payments
- Technical requirements for grid services

### Access and Distribution

**Stakeholder Access:**
> "through consultation company now"

This means: End clients access through the consultation company, not directly.

### User Interface Requirements

**Scope:**
> "Optimization engine, REST API and lets use this opportunity to define some web-based interface too."

**Deliverables:**
1. **REST API** - HTTP API for programmatic access to optimization engine
2. **Python Client Library** - Public Python package for easy API integration
3. **Web Interface** - Browser-based UI for interactive configuration and visualization

**Web Interface Must Include:**
> "the web based interface should include this [comparison mode]"

The web interface should support comparing multiple site configurations side-by-side.

**Python Client Requirements:**
- Separate public package installable via pip
- Auto-generated from OpenAPI specification (ensures API compatibility)
- Type-safe interface with proper type hints
- Convenience methods for common workflows
- Support for async operations (long-running optimizations)
- Comprehensive documentation and examples

**Python Client Design Philosophy:**
- Users work with **high-level device abstractions** (Battery, PV, CHP, HeatAccumulator)
- Users configure device parameters (capacity, power, efficiency)
- Users do NOT need to understand internal flow systems or material balance
- API hides implementation complexity (flows, ports, physics rules)
- Simple, intuitive interface for energy consultants

## Document Structure and Style

### Style Requirements

**Reference Document:** ČEPS MAF 2023 (Czech transmission system operator's resource adequacy assessment)

**User's Instruction:**
> "I think we can create this in Latex. Please create latex project here in subfolder."
> "Currently the document feels quite heavy. Could you make it little less like book or paper and more like e.g. ČEPS's MAF 2023?"

### Document Format Progression

**Desired Structure:**
> "I think the document should start as PRD and slowly through FRD go deeper to technical specification."

This means:
1. Start at **PRD** (Product Requirements Document) level - high-level business requirements
2. Progress through **FRD** (Functional Requirements Document) - detailed functional specifications
3. End with **Technical Specification** - implementation details

### Proposed Document Structure

Based on initial discussion, the structure should include:

#### 1. Executive Summary (1-2 pages)
- Purpose and value proposition
- Target users and use cases
- Key capabilities overview
- Expected business outcomes

#### 2. Product Requirements (PRD Level)

**2.1 Business Context**
- Market need and opportunity
- Target market: Czech Republic energy sector
- User personas (consultation company analysts)
- Success criteria

**2.2 Core Use Cases**
- UC1: Battery + PV sizing for commercial facility
- UC2: CHP + heat storage optimization for industrial site
- UC3: Multi-technology comparison for investment decision
- UC4: Grid service revenue optimization (aFRR)

**2.3 Product Capabilities**
- Optimization objectives (cost minimization, ROI maximization)
- Supported technologies
- Economic outputs
- Scenario comparison
- CZ market specifics

#### 3. Functional Requirements (FRD Level)

**3.1 Optimization Engine**
- Technology sizing optimization
- Operational optimization
- Multi-material flows
- Economic objective functions
- Grid service constraints and revenues

**3.2 Input Data Management**
- Load profiles
- Generation profiles
- Market prices
- Technology parameters
- Site constraints

**3.3 Economic Analysis**
- Investment costs (with economies of scale)
- Operating costs
- Revenue streams
- Financial metrics
- Sensitivity analysis

**3.4 Results & Reporting**
- Optimal sizing recommendations
- Economic summary
- Operational profiles (sample days/weeks)
- Scenario comparison matrix
- Export capabilities

**3.5 REST API**
- Project management endpoints
- Optimization job submission
- Results retrieval
- Scenario management

**3.6 Web Interface**
- Project configuration wizard
- Technology selection
- Data upload
- Optimization execution
- Results visualization and comparison
- Report generation

#### 4. Technical Specification
- Architecture overview
- Domain model
- Optimization engine details
- API specification
- Web application
- Data model

#### 5. Czech Market Specifics
- Electricity market details
- Grid services (aFRR)
- Regulatory framework

#### 6. Examples & Validation Cases
- Example 1: Commercial building with PV + Battery
- Example 2: Industrial facility with CHP + Heat storage
- Example 3: aFRR revenue optimization

#### 7. Implementation Roadmap
- Phase 1: Core optimization engine (current state)
- Phase 2: REST API
- Phase 3: Web interface MVP
- Phase 4: Advanced features

#### 8. Appendices
- Glossary of terms
- Mathematical formulation
- Technology parameter references
- CZ market data sources

## Key Documentation Principles

### What to Emphasize

1. **Economic Focus:** The primary output is economic analysis for bank loan applications
2. **Sizing Optimization:** Optimal parameters are the key deliverable, not operational schedules
3. **Flexibility in Scope:** Document capabilities we can deliver, not just current implementation
4. **CZ Market Integration:** Specific tariffs, fees, and market rules for Czech Republic
5. **Scenario Comparison:** Must support comparing multiple configurations

### What to De-emphasize

1. **Time Series Details:** Operational flows are secondary - shown mainly for illustration
2. **Monte Carlo:** Not part of this software - it's the wrapper around the optimization
3. **Database Code:** Data access will be completely redesigned per CLAUDE.md

### Example Use Cases to Include

**Example 1: Commercial Building with PV + Battery**
- Input parameters for a typical commercial building
- Optimization setup showing sizing as decision variables
- Expected results format
- Economic analysis output (NPV, IRR, ROI)

**Example 2: Industrial Facility with CHP + Heat Storage**
- Dual load profiles (electricity + heat)
- Multi-material optimization
- Technology parameters
- Economic comparison vs baseline

**Example 3: aFRR Revenue Optimization**
- Battery sizing for grid services
- aFRR market parameters
- Revenue breakdown (capacity vs energy payments)
- Impact on IRR

## Technology Capabilities to Document

### Supported Device Types

1. **Battery Storage**
   - Size (kWh) - decision variable
   - Power (kW) - decision variable
   - Efficiency
   - Degradation modeling
   - Charge/discharge scheduling

2. **Photovoltaics (PV)**
   - Capacity (kWp) - decision variable
   - Location-based generation profiles
   - Tilt and azimuth optimization
   - CAPEX with economies of scale

3. **Combined Heat and Power (CHP)**
   - Electrical capacity - can be decision variable
   - Electrical efficiency (e.g., 40%)
   - Thermal efficiency (e.g., 45%)
   - Heat-to-power ratio
   - Operational scheduling

4. **Heat Accumulator**
   - Thermal capacity (kWh) - decision variable
   - Heat losses
   - Charge/discharge profiles

5. **Grid Connection**
   - Maximum power limits
   - Tariff structure (CZ-specific)
   - Import/export scheduling
   - Reserved capacity fees

### Optimization Features

**Decision Variables (what can be optimized):**
- Technology capacities (kW, kWh, kWp)
- Technology powers where applicable
- Operational schedules (hourly dispatch)
- Grid service participation levels

**Objective Functions:**
- Minimize total cost
- Maximize NPV
- Maximize IRR
- Custom economic objectives

**Constraints:**
- Energy balance (supply = demand every hour)
- Technology capacity limits
- Storage state-of-charge dynamics
- Grid connection limits
- Operational constraints (min run times, ramp rates)
- Site-specific constraints (space, regulations)

### Economic Model Details

**CAPEX (Investment Costs):**
- Equipment costs with economies of scale: C = a × P^b where b < 1
- Installation and commissioning
- Grid connection upgrades (if needed)
- Project development costs

**OPEX (Operating Costs):**
- Fixed O&M (EUR/kW/year)
- Variable O&M (EUR/MWh)
- Fuel costs (gas, biomass)
- Grid charges:
  - Reserved capacity fee (CZK/kW/month): 200-400 CZK/kW/month typical for commercial
  - Distribution tariff (CZK/kWh)
  - Electricity tax
- Degradation and replacement reserves

**Revenues:**
- Energy cost savings (vs baseline grid purchase)
- Electricity sales (feed-in, if applicable)
- aFRR capacity payments (CZK/MW/h)
- aFRR energy payments (CZK/kWh when activated)
- Peak demand charge reduction

**Financial Metrics:**
- **NPV:** Net present value at specified discount rate (typically 5-8%)
- **IRR:** Internal rate of return
- **ROI:** Return on investment (%)
- **Payback Period:** Simple and discounted
- **LCOE:** Levelized cost of energy (EUR/kWh)

## Czech Republic Market Specifics

### Electricity Tariffs

**Components to Document:**
1. **Reserved Capacity Fee:**
   - Monthly charge based on maximum subscribed power
   - Typical range: 200-400 CZK/kW/month for commercial
   - Unit: CZK/kW/month

2. **Distribution Tariff:**
   - Energy-based charge (CZK/kWh)
   - Varies by voltage level

3. **Time-of-Use Rates:**
   - Different prices for high/low/off-peak periods

4. **Electricity Tax:**
   - 28.30 CZK/MWh (2024 value)

### aFRR Market

**Key Parameters:**
- **Minimum Bid:** 1 MW
- **Capacity Payment:** Market-based, typically 50-150 CZK/MW/h
- **Energy Payment:** Spot price + premium when activated
- **Availability:** Must be available for contracted hours
- **Technical Requirements:**
  - 5-minute response time
  - 15-minute duration capability

### Regulatory Framework
- Energy Act (458/2000 Sb.)
- Grid connection rules (PPDS)
- Metering requirements for generators
- Support schemes (if applicable)

## System Components

### 1. Optimization Engine (Core)

**Current State:** Active development

**Capabilities to Document:**
- Multiple optimization solver backends
- Solver-independent architecture
- Type-safe device models
- Multi-material flow optimization

**Example Configuration:**
```
Optimization Variables:
- PV capacity: 0-500 kWp
- Battery power: 0-250 kW
- Battery capacity: 0-1000 kWh

Optimization runs for 20-year economic horizon
Results: Optimal sizing + economic metrics
```

### 2. REST API (Planned)

**Purpose:** Integration with external Monte Carlo frameworks and client systems

**Key Endpoints:**
- Project creation and management
- Optimization job submission
- Results retrieval
- Scenario comparison
- Data export

### 3. Python Client Library (Planned)

**Purpose:** Programmatic access to optimization API for integration into client workflows

**Package Name:** `site-calc-client` (pip installable)

**Example Usage:**
```python
from site_calc_client import Client, Battery, PV, Site

# Initialize client
client = Client(api_key="...")

# Create site with devices (high-level abstractions)
site = Site(name="Commercial Building")
site.add_device(Battery(name="Battery1", max_capacity_kWh=1000, max_power_kW=250))
site.add_device(PV(name="PV1", max_capacity_kWp=500))

# Set load profile and prices
site.set_load_profile(electricity_load)  # pandas Series or array
site.set_prices(electricity_prices)

# Configure optimization
optimization = client.create_optimization(
    site=site,
    objective="maximize_npv",
    time_horizon_years=20,
    discount_rate=0.06
)

# Run optimization (async for long-running jobs)
result = optimization.run()

# Access results (high-level, no flows or internal details)
print(f"Optimal PV: {result.get_device_capacity('PV1')} kWp")
print(f"Optimal Battery: {result.get_device_capacity('Battery1')} kWh")
print(f"NPV: {result.npv} EUR")
print(f"IRR: {result.irr * 100}%")
```

**Key Features:**
- Device-oriented API (no flow/port abstractions exposed)
- Type-safe with proper hints
- Pandas integration for time series data
- Simple result access methods
- Async support for long optimizations

### 4. Web Interface (Planned)

**Purpose:** Interactive configuration and visualization for consultants

**Must Include:**
- Project configuration wizard
- Technology selection (checkboxes for Battery, PV, CHP, etc.)
- Parameter input forms
- Optimization execution and monitoring
- Results visualization
- **Side-by-side scenario comparison** (critical requirement)
- Report generation (PDF, Excel)

## Output Requirements

### Primary Outputs (Critical)

1. **Optimal Sizing Recommendations**
   - Technology capacities (kW, kWh, kWp)
   - Specific values for each technology
   - Confidence in recommendations

2. **Economic Summary**
   - Total CAPEX
   - Annual OPEX
   - Annual revenues
   - NPV at specified discount rate
   - IRR
   - ROI
   - Simple payback period
   - Discounted payback period

3. **Scenario Comparison Matrix**
   - Side-by-side comparison of different configurations
   - Easy identification of best option
   - Sensitivity to key parameters

### Secondary Outputs (For Illustration)

1. **Operational Profiles**
   - Sample days or weeks showing typical operation
   - Not the primary deliverable
   - Used to illustrate and validate results

2. **Sensitivity Analysis**
   - Impact of price variations
   - Impact of technology cost changes
   - Impact of degradation assumptions

### Export Formats

- **PDF:** Executive summary + detailed analysis
- **Excel:** Time series data, economic tables
- **JSON/API:** For programmatic access and integration

## Integration Context

### External Monte Carlo Framework

**Important Clarification:**
> "It is worth noting that monte carlo is not part of this software, this is just a optimization in the bigger loop."

**What This Means:**
- External system calls this optimization repeatedly
- Each call = one scenario with specific price assumptions
- External system aggregates results across scenarios
- This platform provides deterministic optimization for single scenario

**Document Should State:**
- Platform designed to integrate with Monte Carlo frameworks
- Efficient single-scenario optimization
- REST API enables batch processing
- Results format supports aggregation

### Typical Workflow

1. External system generates price scenario (1 of N scenarios)
2. Calls optimization API with:
   - Load profiles
   - Technology parameters
   - Price forecasts for this scenario
   - Site constraints
3. Platform returns:
   - Optimal sizing
   - Economic metrics
   - Operational summary
4. Repeat for N scenarios
5. External system performs statistical analysis across scenarios

## Document Style Guidelines (from ČEPS MAF 2023)

### Visual Style

**Key Characteristics:**
- Clean, modern layout (not academic)
- Professional/corporate feel
- Blue color scheme (primary color: RGB 0,102,161)
- Sans-serif fonts throughout
- Generous white space

### Structural Style

**Section Numbering:**
- Use numbered sections: 1, 1.1, 1.2, 2, 2.1, etc.
- **NO "Chapter" labels** - just section numbers
- Clean hierarchy

### Content Style

**Less Like:**
- Academic paper
- Dense textbook
- Technical manual

**More Like:**
- Professional report
- Executive briefing
- Industry standard

### Visual Elements

**Emphasized:**
- Infographics and diagrams
- Color-coded sections
- Icons for different technologies
- Boxed highlights for key information
- Tables with clear formatting
- Charts and graphs

**De-emphasized:**
- Long paragraphs of dense text
- Footnotes and academic citations
- Overly technical notation

### Typography

- Section headings in blue
- Clean sans-serif font (Helvetica-like)
- Bullet points for lists
- Short paragraphs
- Clear visual hierarchy

## Examples Format

Each example should include:

### Structure

1. **Scenario Description**
   - Facility type
   - Load characteristics
   - Site constraints

2. **Input Parameters**
   - Specific numerical values
   - Technology options considered
   - Economic parameters

3. **Optimization Setup**
   - Decision variables
   - Constraints
   - Objective function

4. **Expected Results**
   - Optimal sizing (specific numbers)
   - Economic metrics
   - Key insights

5. **Economic Analysis**
   - Investment required
   - Annual savings
   - NPV, IRR, ROI
   - Payback period

### Example Template

**Example: Commercial Office Building**

**Facility:**
- 5,000 m² office building
- Peak electricity demand: 200 kW
- Annual consumption: 600 MWh
- No heat demand (air conditioning only)

**Technologies Evaluated:**
- Rooftop PV: 0-300 kWp
- Battery: 0-200 kW / 0-800 kWh

**Economic Parameters:**
- PV CAPEX: 800 EUR/kWp
- Battery CAPEX: 200 EUR/kWh + 150 EUR/kW
- Grid electricity: 0.15 EUR/kWh average
- Reserved capacity: 300 CZK/kW/month
- Discount rate: 6%
- Project lifetime: 20 years

**Results:**
- Optimal PV: 250 kWp
- Optimal Battery: 150 kW / 600 kWh
- Total CAPEX: 350,000 EUR
- Annual savings: 45,000 EUR
- NPV: 285,000 EUR
- IRR: 12.5%
- Payback: 7.8 years

## Success Criteria

The specification document is successful if:

1. **Business Alignment**
   - Clearly supports bank loan applications
   - Focuses on economic analysis
   - Emphasizes optimal sizing over operational details

2. **Technical Clarity**
   - Clearly describes what can be optimized
   - Explains capabilities vs current implementation
   - Provides concrete examples

3. **Market Relevance**
   - CZ market specifics are accurate and complete
   - aFRR integration is clear
   - Tariff structures are well-documented

4. **User Focus**
   - Written for energy consultants
   - Appropriate technical level
   - Professional presentation

5. **Completeness**
   - All key features documented
   - Examples validate concepts
   - Clear implementation roadmap

## Future Considerations

### Potential Expansions

While current focus is CZ market and core features, document should be structured to accommodate:

1. Additional markets (Poland, Slovakia, etc.)
2. Additional technologies (wind, hydrogen, etc.)
3. More complex grid services
4. Direct client access (currently through consultants)

### Version Control

- Version 1.0: Initial specification (current work)
- Future versions should track:
  - Feature additions
  - Market expansions
  - Updated economic parameters
  - New examples

## Key Takeaways

1. **Primary Purpose:** Support bank loan applications with economic analysis
2. **Primary Output:** Optimal sizing + NPV/IRR/ROI
3. **Primary Market:** Czech Republic
4. **Primary User:** Energy consultants at large firms
5. **Primary Style:** Professional report (ČEPS MAF 2023 style)
6. **Primary Scope:** Document capabilities we can deliver
7. **Primary Structure:** PRD → FRD → Technical Specification
8. **Primary Focus:** Economics over operations
9. **Primary Integration:** REST API for Monte Carlo frameworks
10. **Primary Comparison:** Side-by-side scenario comparison

## Document Status

- **Location:** `C:\my_source\site-calc\docs\specification\`
- **Format:** LaTeX (article class)
- **Style:** Modern blue theme, sans-serif fonts
- **Current State:** Structure established, content in progress
- **Next Steps:** Fill in detailed content for each section