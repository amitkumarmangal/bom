# Platform BOM (Bill of Materials)

## 📦 Overview

This repository contains the **Platform Bill of Materials (BOM)** used to centrally manage and govern **dependency versions and build standards** across all projects.

The BOM acts as a **single source of truth** for:

* Approved libraries
* Dependency versions
* Java version
* Build

All child projects **must consume dependencies only via this BOM**.

---

## 🎯 Objectives

* Eliminate dependency version drift
* Enforce consistent builds across teams
* Simplify upgrades and security patching
* Fail fast during CI for violations

---

## 🧱 What This BOM Provides

* Centralized `dependencyManagement`
* Imported third-party BOMs (e.g. Spring Boot)
* Approved internal and external libraries
* Plugin version alignment

---

## 🔐 Dependency Governance Rules

This BOM enforces the following rules using **Maven Enforcer Plugin**:

| Rule                          | Description                                       |
| ----------------------------- | ------------------------------------------------- |
| Require Dependency Management | All dependencies must be declared in the BOM      |
| No Hardcoded Versions         | Child projects cannot define dependency versions  |
| Fail Fast                     | Build fails immediately on violation              |

---

## 📂 Repository Structure

```
bom/
├── pom.xml        # BOM / Parent POM
└── README.md      # Documentation
```

---

## 🚀 How to Use This BOM

### Option 1: Use as Parent POM (Recommended)

```xml
<parent>
    <groupId>com.business</groupId>
    <artifactId>bom</artifactId>
    <version>1.0-SNAPSHOT</version>
</parent>
```

---

### Option 2: Import as BOM (Alternative)

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.business</groupId>
            <artifactId>Bom</artifactId>
            <version>1.0-SNAPSHOT</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

---

## ✅ Declaring Dependencies in Child Projects

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
    </dependency>
</dependencies>
```

✔ Versions are resolved from the BOM
❌ Versions in child POMs are not allowed

---

## 🔄 Dependency Upgrade Process

1. Update dependency version **only in the BOM**
2. Run full build and tests
3. Release a new BOM version
4. Promote the BOM via CI / repository manager
5. Child projects automatically inherit approved versions

---

## 🏷 Versioning Strategy

The BOM follows **Semantic Versioning**:

| Version | Meaning                      |
| ------- | ---------------------------- |
| MAJOR   | Breaking dependency changes  |
| MINOR   | New approved dependencies    |
| PATCH   | Bug fixes / security updates |

Example:

```
1.0.0 → 1.1.0 → 1.1.1
```

---

## ☕ Supported Java Version

* Java **17**

---

## 🛡 Best Practices

* All new dependencies must be added to the BOM first
* Avoid direct dependency imports in child projects
* Use CI to enforce BOM compliance
* Perform security and license checks at BOM level

---

## 👥 Ownership & Support

This BOM is maintained by the **Platform / Architecture Team**.

For:

* Adding new dependencies
* Version upgrades
* Exceptions or issues

Please raise a Pull Request or contact the platform team.

---

## 📄 License

Internal use only.
Not intended for public distribution.
