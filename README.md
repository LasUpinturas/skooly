<div align="center">
  <h1>🎓 Skooly</h1>
  <p><strong>Modern ERP for African Universities & Schools</strong></p>
  
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

## 🌟 About

**Skooly** is a complete, modern, and open-source ERP system designed specifically for African universities, technical institutes (IUTs), and schools. Built with cutting-edge technologies and optimized for the African context with features like Mobile Money integration, offline-first architecture, and support for the LMD (License-Master-Doctorate) academic system.

### Why Skooly?

- 🚀 **Modern Stack**: Built with Next.js, NestJS, Prisma, and Turborepo
- 🌍 **Africa-First**: Mobile Money payments (MTN, Orange), offline support, optimized for limited bandwidth
- 📱 **Mobile-Ready**: Progressive Web App with native mobile app support
- 🔒 **Secure**: QR code authentication, anti-fraud document generation, comprehensive audit trails
- 🎯 **Complete**: Manages students, teachers, grades, attendance, finances, and more
- 💰 **Cost-Effective**: Open-source with self-hosting option

---

## ✨ Features

### 📚 Academic Management
- **Student Management** - Complete student lifecycle from admission to graduation
- **LMD Grading System** - Full support for Cameroon's License-Master-Doctorate system
- **Course Management** - Departments, programs, UE (Teaching Units), EC (Course Elements)
- **Automatic Grade Calculations** - Credits, weighted averages, validations, compensations
- **Deliberations & Transcripts** - Digital PV generation, official transcripts with QR codes

### 📍 Attendance Systems
- **QR Code Attendance** - Dynamic QR codes with geolocation verification (anti-fraud)
- **Teacher Tracking** - Course session management and reporting
- **Automated Alerts** - SMS/email notifications for absences exceeding thresholds
- **Real-time Statistics** - Attendance rates per student, course, and program

### 💰 Financial Management
- **Mobile Money Integration** - MTN Mobile Money and Orange Money APIs
- **Payment Plans** - Installment payments with automated reminders
- **Scholarships & Discounts** - Excellence, social, and government scholarships
- **Automated Receipts** - PDF receipts with QR code verification sent via SMS/email

### 📅 Scheduling
- **Smart Timetables** - Automated scheduling with conflict detection
- **Resource Management** - Classrooms, labs, equipment allocation
- **Teacher Availability** - Manage permanent staff and part-time lecturers
- **Real-time Updates** - Instant notifications for schedule changes

### 📜 Document Generation
- **Official Documents** - Certificates, transcripts, diplomas with QR authentication
- **Anti-Fraud Security** - Unique numbering, QR verification portal for employers
- **Online Requests** - Students request documents online with payment and tracking

### 📱 Communication
- **Multi-Channel** - SMS (Twilio/Infobip), WhatsApp Business API, Email, Push notifications
- **Automated Messages** - Results publication, payment reminders, schedule changes
- **User Portals** - Dedicated dashboards for students, teachers, administration, and parents

### 🎓 Additional Modules
- **Internship Management** - Applications, conventions, tracking, evaluations
- **Thesis & Projects** - Proposal submission, advisor assignment, defense scheduling
- **Library** - Catalog, loans, reservations, digital resources
- **E-Learning** - Online courses, quizzes, virtual classrooms
- **Alumni Network** - Job board, mentorship, career tracking

---

## 🛠️ Tech Stack

### Frontend
```
• Next.js 16        - React framework with App Router
• TypeScript 5.9    - Type safety
• TailwindCSS       - Utility-first CSS
• shadcn/ui         - Beautiful UI components (Radix UI)
• React Hook Form   - Form handling
• Zod               - Schema validation
• Recharts          - Data visualization
```

### Backend
```
• NestJS 11         - Progressive Node.js framework
• Prisma ORM        - Type-safe database client
• PostgreSQL        - Primary database
• Redis             - Caching & sessions
• Passport.js       - Authentication (JWT)
• Bull              - Job queues
```

### Infrastructure
```
• Turborepo         - High-performance monorepo
• pnpm              - Fast, disk-efficient package manager
• Docker            - Containerization
• GitHub Actions    - CI/CD
```

### Integrations
```
• MTN Mobile Money  - Mobile payments (Cameroon)
• Orange Money      - Mobile payments (Cameroon)
• Twilio/Infobip    - SMS notifications
• WhatsApp API      - Messaging
• SendGrid          - Email delivery
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** 18+ 
- **pnpm** 8+
- **PostgreSQL** 15+
- **Redis** 7+

### Quick Start

```bash
# Clone the repository
git clone https://github.com/yourusername/skooly.git
cd skooly

# Install dependencies
pnpm install

# Setup environment variables
cp .env.example .env
# Edit .env with your configuration

# Setup database
pnpm db:push

# Start development servers (all apps)
pnpm dev
```

The services will be available at:
- 🌐 **Web App**: http://localhost:3000
- 🔧 **API**: http://localhost:3001
- 📚 **Docs**: http://localhost:3002

### Project Structure

```
skooly/
├── apps/
│   ├── web/              # Next.js frontend application
│   ├── api/              # NestJS backend API
│   ├── docs/             # Documentation site
│   └── mobile/           # React Native app (optional)
├── packages/
│   ├── database/         # Prisma schema & client
│   ├── types/            # Shared TypeScript types
│   ├── ui/               # Shared UI components
│   └── utils/            # Shared utilities
└── docs/                 # Markdown documentation
```

---

## 📖 Documentation

Comprehensive documentation is available in the [`/docs`](./docs) directory:

- **[Getting Started Guide](./docs/23-GETTING-STARTED.md)** - Installation and setup
- **[Technical Architecture](./docs/20-TECHNICAL-ARCHITECTURE.md)** - System design and stack
- **[API Documentation](./apps/api/README.md)** - Backend API reference
- **[Database Schema](./docs/21-DATABASE-SCHEMA.md)** - Data models and relationships
- **[Deployment Guide](./docs/DEPLOYMENT.md)** - Production deployment

---

## 🌍 Cameroon Context Features

### Mobile Money
```typescript
// MTN Mobile Money integration
import { MTNMoMoService } from '@skooly/payments';

const payment = await momo.requestToPay({
  amount: 100000, // FCFA
  phoneNumber: '237670000000',
  message: 'Tuition fees - Semester 1'
});
```

### Offline-First
- Local caching with IndexedDB
- Automatic sync when connection restored
- Queue for pending actions
- Works with unstable internet

### LMD System
- Full support for Licence-Master-Doctorat
- Credit-based (ECTS) calculations
- Semester and annual deliberations
- Compensation between UE

---

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](./CONTRIBUTING.md) for details.

### Development Workflow

```bash
# Create a feature branch
git checkout -b feature/amazing-feature

# Make your changes and commit
git commit -m "feat: add amazing feature"

# Push and open a PR
git push origin feature/amazing-feature
```

### Commit Convention

We follow [Conventional Commits](https://www.conventionalcommits.org/):
- `feat:` - New features
- `fix:` - Bug fixes
- `docs:` - Documentation changes
- `chore:` - Maintenance tasks
- `refactor:` - Code refactoring

---

## 📜 License

This project is licensed under the **MIT License** - see the [LICENSE](./LICENSE) file for details.

---

## 🙏 Acknowledgments

- Built for the **IUT de Douala** and African universities
- Inspired by modern SaaS products like Vercel, Linear, and Notion
- Community-driven and open-source

---

## 📞 Support & Community

- 🐛 **Bug Reports**: [GitHub Issues](https://github.com/yourusername/skooly/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/yourusername/skooly/discussions)
- 📧 **Email**: support@skooly.io
- 🌐 **Website**: https://skooly.io

---

## 🎯 Roadmap

### v1.0 (Current - MVP)
- [x] Student & teacher management
- [x] LMD grading system
- [x] QR code attendance
- [x] Mobile Money payments
- [x] Document generation
- [ ] Complete testing
- [ ] Production deployment

### v1.1 (Next)
- [ ] Mobile app (React Native)
- [ ] E-learning platform
- [ ] Advanced analytics
- [ ] Multi-tenant SaaS mode

### v2.0 (Future)
- [ ] AI-powered features
- [ ] Blockchain diplomas
- [ ] Advanced BI & reporting
- [ ] Integration marketplace

---

## ⭐ Star History

If you find Skooly useful, please consider giving it a star ⭐

---

<div align="center">
  <p>Built with ❤️ for African Education</p>
  <p>
    <a href="https://skooly.io">Website</a> •
    <a href="https://docs.skooly.io">Documentation</a> •
    <a href="https://twitter.com/skooly">Twitter</a>
  </p>
</div>
