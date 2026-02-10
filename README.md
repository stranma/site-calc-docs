n# Site-Calc Documentation Project

**Purpose:** Shared documentation, specifications, and design documents for the Site-Calc energy optimization platform.

This documentation project serves as the **source of truth** for API contracts, technical specifications, and architectural decisions that span multiple packages in the Site-Calc meta-repository.

---

## 📚 Documentation Structure

### **`api/` - REST API Specifications**

**Purpose:** Contract definitions for the Site-Calc optimization API

| Document | Description | Audience |
|----------|-------------|----------|
| **OPTIMIZATION_API_SPEC.md** | Complete REST API specification | Server developers, all client implementers |
| **OPTIMIZATION_API_SPEC.pdf** | PDF version (for stakeholders) | Non-technical reviewers, presentations |

**Who uses this:**
- Server developers implementing the API
- Client library developers (Python, JavaScript, etc.)
- Anyone building integrations with Site-Calc
- DevOps testing with curl/Postman

**Note:** Python SDK users should see package-specific documentation:
- Operational client: `../client-operational/docs/OPERATIONAL_CLIENT_SPEC.md`
- Investment client: `../client-investment/docs/INVESTMENT_CLIENT_SPEC.md`

---


### **`specification/` - Formal Product Specification**

**Purpose:** LaTeX-formatted formal specification document

**Contents:**
- Executive summary
- Product overview and use cases
- Functional requirements
- Technical architecture
- Czech market specifics
- Mathematical formulations
- Technology stack

**Build:**
```bash
cd specification/
pdflatex main.tex
```

**Output:** `main.pdf` - Formal specification document for stakeholders

---

### **Root-Level Documents**

#### **SPECIFICATION_REQUIREMENTS.md**
**Purpose:** Requirements gathering and context for creating the product specification

**Contains:**
- Target audience (consultation companies, bank loan applications)
- Primary use cases (battery sizing with renewables)
- Feature scope (devices, materials, economic models)
- Czech market specifics (distribution tariffs, aFRR rules)
- Interface requirements (REST API, Python client, web UI)

**Audience:** Product managers, technical writers, specification authors

---

## 🎯 Document Relationships

### **Hierarchy:**

```
Product Requirements (SPECIFICATION_REQUIREMENTS.md)
    ↓
Formal Specification (specification/main.pdf)
    ↓
REST API Contract (api/OPTIMIZATION_API_SPEC.md)
    ↓
├─ Server Implementation (../server/)
├─ Python SDK - Operational (../client-operational/)
└─ Python SDK - Investment (../client-investment/)
```

### **When to Use Which Document:**

| I want to... | Read this |
|--------------|-----------|
| Implement the API server | `api/OPTIMIZATION_API_SPEC.md` |
| Use the Python SDK (operational) | `../client-operational/docs/OPERATIONAL_CLIENT_SPEC.md` |
| Use the Python SDK (investment) | `../client-investment/docs/INVESTMENT_CLIENT_SPEC.md` |
| Understand product requirements | `SPECIFICATION_REQUIREMENTS.md` |
| Review formal specification | `specification/main.pdf` |

---

## 🔄 Maintenance Guidelines

### **When to Update These Documents:**

1. **API Spec (`api/OPTIMIZATION_API_SPEC.md`)**
   - When adding new endpoints
   - When changing request/response schemas
   - When modifying device types or properties
   - When updating validation rules

2. **Formal Spec (`specification/`)**
   - When product features change significantly
   - When updating for stakeholder presentations
   - Quarterly reviews for investment clients

3. **Requirements Doc (`SPECIFICATION_REQUIREMENTS.md`)**
   - When target audience changes
   - When adding new use cases
   - When market requirements evolve

### **Versioning:**

- API spec includes version number at top: `Version: 1.0`
- Update `Last Updated` date when making changes

### **Review Process:**

1. **Draft changes** in a branch
2. **Verify consistency** between API spec and client specs
3. **Update related docs** (server docs, client docs)
4. **Get stakeholder approval** for API changes
5. **Merge and tag** major version changes

---

## 📂 Package-Specific Documentation

While this folder contains **shared documentation**, each package has its own specific docs:

### **Server** (`../server/docs/`)
- `SERVER_SPEC.md` - Server architecture and REST API specification
- `IMPLEMENTATION_PLAN.md` - Development roadmap and phase tracking
- `SERVER_STATUS.md` - Current implementation status
- `SITE_CALC2_INTEGRATION.md` - Core library integration guide
- `API_KEY_SETUP.md` - API key creation and management
- `GRAFANA_SETUP.md` - Monitoring dashboard setup

### **Client - Investment** (`../client-investment/docs/`)
- `INVESTMENT_CLIENT_SPEC.md` - Python SDK usage guide
- `MCP_SERVER_SPEC.md` - MCP server for LLM-driven investment planning (17 tools)

### **Client - Operational** (`../client-operational/docs/`)
- `OPERATIONAL_CLIENT_SPEC.md` - Python SDK usage guide

---

## 🔒 Security Note

**IMPORTANT:** This documentation project is part of the private meta-repository.

- ✅ **Can be shared:** Product specifications, use case descriptions
- ⚠️ **Internal only:** API implementation details, bidding strategies
- 🔒 **Private:** Anything referencing `site-calc-core` internals

See `../SECURITY.md` for complete security policy.

---

## 🤝 Contributing

### **For Documentation Updates:**

1. Check which document needs updating (see hierarchy above)
2. Make changes in a feature branch
3. Ensure consistency across related documents:
   - API spec ↔ Client specs
   - Requirements ↔ Formal spec
4. Update "Last Updated" date
5. Submit for review

### **For New Documents:**

- Add to appropriate folder (`api/`, `bid_calc/`, etc.)
- Update this README with description
- Add cross-references from related documents
- Include audience, purpose, and maintenance notes

---

## 📞 Contact

**Questions about:**
- API specification → Server team
- Product requirements → Product management
- Formal specification → Technical writing
- Bidding optimization → Optimization team

---

**Last Updated:** 2026-02-06
**Maintained by:** Site-Calc core team