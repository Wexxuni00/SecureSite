# SecureSite: Encrypted Document Portal for Construction Teams  
**SecureSite** is a secure front-end portal for managing and sharing sensitive construction documents between stakeholders. Built for performance and security, SecureSite focuses on encrypted file handling, fine-grained access controls, and a responsive UX suitable for desktop and mobile users.

# Table of Contents
- [Overview](#overview)
- [Key Features](#key-features)
- [Tech Stack](#tech-stack)
- [How It Works](#how-it-works)
- [Security Model](#security-model)
- [Performance & Optimization](#performance--optimization)
- [Testing & QA](#testing--qa)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

## Overview
SecureSite provides a hardened interface for uploading, viewing, and sharing sensitive documents (plans, contracts, compliance docs) while enforcing encryption, multi-factor authentication, and performance-optimized delivery. The portal aims to minimize exposure risk while preserving a responsive, accessible experience for field and office users.

## Key Features

<details>
  <summary><b>Client-Side & Server-Side Encryption</b></summary>
  <ul>Optional client-side encryption (before upload) with AES-256; server-side encryption using KMS-managed keys for stored objects.</ul>
</details>

<details>
  <summary><b>Two-Factor Authentication (2FA)</b></summary>
  <ul>Supports TOTP (Authenticator apps) and SMS/Email OTP as additional verification for sensitive actions.</ul>
</details>

<details>
  <summary><b>Granular Permissions & Audit Trails</b></summary>
  <ul>Policy-driven access (project-level, folder-level) with immutable audit logs for downloads, shares, and edits.</ul>
</details>

<details>
  <summary><b>Fast Document Delivery</b></summary>
  <ul>Signed, time-limited download URLs and CloudFront CDN caching with origin access control for secure, low-latency access.</ul>
</details>

<details>
  <summary><b>Responsive & Accessible UI</b></summary>
  <ul>Mobile-first design with ARIA support, keyboard navigation, and accessibility checks using Axe and Lighthouse.</ul>
</details>

<details>
  <summary><b>Performance Optimizations</b></summary>
  <ul>Lazy-loaded Angular modules, chunked file uploads, preview thumbnails, and smart caching strategies.</ul>
</details>

## Tech Stack
- **Frontend:** Angular + TypeScript + NgRx + Angular Material / Tailwind  
- **Backend:** Spring Boot (Java) REST APIs  
- **Storage & CDN:** AWS S3 (SSE-KMS) + CloudFront (signed URLs)  
- **Database:** PostgreSQL (metadata), Redis (caching)  
- **Encryption:** AES-256 (client optional), AWS KMS for server-side key management  
- **Auth:** OAuth2 / JWT + TOTP 2FA (RFC 6238)  
- **Testing:** Jasmine & Karma (unit), Cypress (E2E)  
- **CI/CD:** GitHub Actions → Jenkins or native GitHub deploy workflows

## How It Works
1. **Auth & Access:** Users authenticate via OAuth2 + 2FA; roles and policies determine accessible resources.  
2. **Upload Flow:** Files optionally encrypted client-side, then uploaded in chunks to S3 using signed URLs; server stores metadata and audit entry.  
3. **Download Flow:** Users request documents; server verifies permissions, logs request, and issues short-lived signed CloudFront or S3 URL.  
4. **Preview & Thumbnails:** Background workers generate thumbnails and text-indexing for search while preserving encryption boundaries.  
5. **Sharing & Revocation:** Shares are permissioned links or user invitations; owners can revoke access which invalidates future signed URLs.

## Security Model
- **Least Privilege:** RBAC and policy-based authorization enforced server-side.  
- **Encryption-in-Transit & At-Rest:** TLS for all network traffic; S3 objects encrypted with KMS-managed keys.  
- **Client-Side Encryption (Optional):** For high-sensitivity documents, public-key crypto can be used to encrypt before upload.  
- **Key Management:** AWS KMS for rotation and auditing of keys.  
- **Audit & Monitoring:** Immutable logs stored in a WORM-enabled store or CloudWatch + S3 with lifecycle retention.  
- **Vulnerability Hardening:** Dependency scanning (Snyk), static analysis, and periodic pentesting.

## Performance & Optimization
- **Lazy Loading:** Route-level lazy modules to reduce initial bundle size.  
- **Chunked Uploads:** Resumable, multi-part uploads for large files.  
- **Caching:** CDN + short-lived signed URLs to balance security with performance.  
- **Accessibility & UX:** Lighthouse and Axe scores integrated into the CI pipeline; keyboard-first navigation and reduced-motion support.

## Testing & QA
- **Unit Tests:** Jasmine + Karma for Angular components and services.  
- **E2E Tests:** Cypress covering upload/download, permission flows, and 2FA.  
- **Security Tests:** Automated SAST/DAST and dependency vulnerability checks in CI.  
- **Accessibility Tests:** Axe-core integration in CI to prevent regressions.

## Deployment
- Build and test via GitHub Actions; container images pushed to ECR / container registry.  
- Deploy backend services to ECS/EKS behind an ALB with strict security groups.  
- Infrastructure-as-code via Terraform; secrets managed by AWS Secrets Manager.  
- Canary or blue/green deployments recommended for production releases.

## Contributing
Contributions welcome from security engineers, frontend devs, and ops engineers. Please follow `CONTRIBUTING.md` for local setup, testing, and PR guidelines. Security-sensitive changes require an accompanying threat model and test plan.

## License
This project is licensed under the **Apache 2.0 License**.
