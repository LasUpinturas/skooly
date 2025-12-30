<div align="center">
  <h1>🎓 Skooly</h1>
  <p><strong>The Open Source ERP for African Universities & Schools</strong></p>
  
  <p>
    <a href="#features">Features</a> •
    <a href="#tech-stack">Tech Stack</a> •
    <a href="#getting-started">Getting Started</a> •
    <a href="#documentation">Docs</a> •
    <a href="#contributing">Contributing</a>
  </p>

  <p>
    <img src="https://img.shields.io/badge/TypeScript-5.9-blue?style=flat-square&logo=typescript" alt="TypeScript" />
    <img src="https://img.shields.io/badge/Next.js-16-black?style=flat-square&logo=next.js" alt="Next.js" />
    <img src="https://img.shields.io/badge/NestJS-11-red?style=flat-square&logo=nestjs" alt="NestJS" />
    <img src="https://img.shields.io/badge/Turborepo-2.7-orange?style=flat-square&logo=turborepo" alt="Turborepo" />
    <img src="https://img.shields.io/badge/License-MIT-yellow?style=flat-square" alt="License" />
  </p>
</div>

---

## 🦅 The Manifesto

**We refuse to accept that our universities, which train tomorrow's elite, are managed by yesterday's tools.**

Skooly is not just school management software. It is an **Infrastructure**.
It's built on a radical philosophy: **Traceability is sacred**. We don't just store the current state (e.g., "Registered"), we store the facts (Events) that led there.

> "I don't ship code. I ship administrative dignity."

[📖 Read the full Vision](./docs/1-concepts/01-vision.md)

---

## ✨ Features

### 🧱 Core (Community Edition - MIT)
*   **Identity Management**: Strict separation between User (Login) and Partner (Profile).
*   **Academic Structure**: Full support for LMD (Licence-Master-Doctorat) hierarchy.
*   **Enrollment Workflow**: From Draft to Validated to Paid.
*   **Basic Attendance**: QR Code scanning logic.

### 💎 Enterprise (Premium Extensions)
*   **💰 Finance & Mobile Money**: Native integration with MTN/Orange Money. Double-entry accounting.
*   **📍 Anti-Fraud Attendance**: Rotating TOTP QR Codes + Geolocation fencing.
*   **📊 Grades & Deliberations**: Complex grade calculation, compensation rules, and locked jury minutes.
*   **📜 Secure Documents**: Diplomas signed with private keys and verifiable via QR.
*   **🧠 AI Assistant**: Dropout prediction and Computer Vision for document fraud.

---

## 🛠️ Tech Stack

We chose robustness over hype.

*   **Monorepo**: Turborepo (Isolated builds)
*   **Backend**: NestJS (Modular Architecture, strong injection)
*   **Frontend**: Next.js App Router (Server Components for performance)
*   **Database**: PostgreSQL + Prisma (Type-safety from DB to UI)
*   **Infrastructure**: Docker + Redis (Queue management)

[📖 Read the Architecture Decision Record](./docs/3-technical/20-stack.md)

---

## 🚀 Getting Started

We have prepared a specific journey for developers to understand the "Skooly Way" (Event-Driven).

### 1. Prerequisites
- Node.js 18+
- pnpm 8+
- Docker

### 2. Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/skooly.git
cd skooly

# Install dependencies
pnpm install

# Start Infrastructure (Postgres, Redis)
docker-compose up -d

# Sync Database
pnpm db:push

# Launch Development Server
pnpm dev
```

### 3. Your First Step
Don't just dive into the code. Read the **Developer Journey** guide first.

[🗺️ Start the Developer Journey](./docs/4-guides/DEV-JOURNEY.md)

---

## 📖 Documentation Structure

Our documentation is authoritative and structured in 4 layers:

1.  **[Concepts](./docs/1-concepts)**: Vision, Architecture, Business Model.
2.  **[Specifications](./docs/2-specs)**: Deep dive into every module (Finance, Grades, AI...).
3.  **[Technical](./docs/3-technical)**: Code structure, Odoo translation guide.
4.  **[Guides](./docs/4-guides)**: Onboarding and tutorials.

[📂 Browse full documentation](./docs/00-INDEX.md)

---

## 🤝 Contributing

We welcome contributions!
Please read our [Technical Architecture](./docs/3-technical/01-project-structure.md) before submitting a PR.
We enforce strict boundaries between modules.

---

## 📜 License

This project is licensed under the **MIT License**.
See [Open Core Strategy](./docs/1-concepts/05-open-core-strategy.md) for details on the Enterprise edition.

---

<div align="center">
  <p>Built with ❤️ for African Education</p>
  <p>
    <a href="https://skooly.io">Website</a> •
    <a href="https://twitter.com/skooly">Twitter</a>
  </p>
</div>
