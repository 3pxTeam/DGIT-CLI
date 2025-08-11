# DGit - Design File Version Control System

<div align="center">

![DGit Logo](https://img.shields.io/badge/DGit-Design%20Git-blue?style=for-the-badge&logo=git)

**Intelligent version control for design files** 🎨

[![Go Version](https://img.shields.io/badge/Go-1.21+-00ADD8?style=flat&logo=go)](https://golang.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg?style=flat)](LICENSE)
[![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen?style=flat)](https://github.com)

</div>

## 🎯 Project Overview

**DGit** is a specialized version control system designed for design files (.psd, .ai, .sketch, .fig, etc.). It addresses the limitations of traditional Git when working with large binary design files through intelligent compression and metadata tracking.

### 💡 Key Problems Solved

- **Performance**: Significantly faster commits for large design files
- **Storage Efficiency**: Advanced compression reduces storage requirements by up to 88%
- **Design-Aware**: Tracks meaningful changes like layer count, dimensions, and color modes
- **User Experience**: Designer-friendly interface with visual feedback

## ⚡ Core Features

### 🚀 Smart Compression System
```
User Action → Fast Compression → Immediate Response
              ↓
         Background Optimization (when idle)
              ↓
         Long-term Storage (automated)
```

### 🎨 Design File Intelligence
```bash
📝 Status Check:
🔄 modified: logo.psd
   📐 Dimensions: 1920×1080 → 2560×1440
   🎨 Layers: 57 → 59 (+2)
   🎯 Color Mode: RGB → CMYK
   💾 File Size: 56MB → 61MB
```

### 🔍 Metadata Tracking
DGit understands your design files and tracks meaningful changes:
- Layer structure modifications
- Canvas size adjustments  
- Color space conversions
- Version compatibility

## 📊 Performance Improvements

### Speed Comparison (56MB PSD file)

| Operation | Traditional Git | DGit | Improvement |
|-----------|----------------|------|-------------|
| Commit | 45 seconds | 0.2 seconds | 225x faster |
| Status Check | 3 seconds | Instant | Real-time |
| File Restore | 15 seconds | 0.1 seconds | 150x faster |

### Storage Efficiency
```
Traditional approach: 560MB (10 versions × 56MB each)
DGit smart compression: 190MB (66% space savings)
```

## 🛠️ Installation

### Prerequisites
- Go 1.21 or higher installed
- Command line access

### Build from Source
```bash
# Clone the repository
git clone https://github.com/your-username/dgit.git
cd dgit

# Build the application
go build -o dgit

# Optional: Install globally
sudo mv dgit /usr/local/bin/
```

### Verify Installation
```bash
dgit --help
```

## 🚀 Quick Start Guide

### 1. Initialize a DGit Repository
```bash
# In your design project folder
dgit init

# Or specify a directory
dgit init /path/to/project
```

### 2. Scan for Design Files
```bash
# See what design files DGit can manage
dgit scan .
```

### 3. Add Files to Staging
```bash
# Add specific files
dgit add logo.psd banner.ai

# Add all design files in current directory
dgit add .

# Add files by pattern
dgit add *.psd
```

### 4. Create Your First Commit
```bash
dgit commit -m "Initial design files"
```

Example output:
```bash
🎨 Creating commit with 2 design files...
📊 Analyzing file metadata...
📦 Applying compression...

✨ Commit created: a1b2c3d4
📝 Initial design files
🎨 Files committed:
   🔵 logo.psd (12 layers, 1920×1080, RGB)
   🟠 banner.ai (8 layers, 1200×800, CMYK)
```

### 5. Check Repository Status
```bash
dgit status
```

### 6. View Commit History
```bash
dgit log
```

### 7. Restore Previous Versions
```bash
# Restore specific file from a version
dgit restore v2 logo.psd

# Restore all files from a version
dgit restore v1
```

## 📁 Project Architecture

```
dgit/
├── main.go                      # Application entry point
├── cmd/                         # Command line interface
│   ├── initCmd.go              # Repository initialization
│   ├── addCmd.go               # File staging
│   ├── commitCmd.go            # Version creation
│   ├── statusCmd.go            # Repository status
│   ├── logCmd.go               # History viewing
│   ├── restoreCmd.go           # File restoration
│   └── scanCmd.go              # File discovery
└── internal/                    # Core business logic
    ├── init/                   # Repository setup
    ├── staging/                # File staging management
    ├── commit/                 # Compression and versioning
    ├── log/                    # History tracking
    ├── restore/                # File restoration
    ├── status/                 # Change detection
    └── scanner/                # File analysis
        ├── photoshop/          # PSD file parser
        └── illustrator/        # AI file parser
```

## 🎨 Supported File Formats

### Full Support (with metadata extraction)
- ✅ **Adobe Photoshop (.psd)** - Layers, dimensions, color mode, bit depth
- ✅ **Adobe Illustrator (.ai)** - Artboards, layers, vector content, color space

### Basic Support (file versioning)
- 🔶 **Sketch (.sketch)** - File tracking and versioning
- 🔶 **Figma (.fig)** - File tracking and versioning  
- 🔶 **Adobe XD (.xd)** - Basic file management
- 🔶 **Affinity Designer (.afdesign)** - File versioning
- 🔶 **Blender (.blend)** - 3D file versioning

### 📋 Planned Enhancements
Enhanced metadata extraction coming soon for:
- Sketch: Symbol libraries, artboard analysis
- Figma: Component systems, design tokens
- Adobe XD: Prototype flows, component states

## 🔧 Configuration

### Custom Settings
Create `.dgit/config` to customize behavior:

```json
{
  "user": {
    "name": "Your Name",
    "email": "your.email@example.com"
  },
  "compression": {
    "level": "balanced",
    "background_optimization": true
  },
  "ui": {
    "show_file_details": true,
    "color_output": true
  }
}
```

### Command Reference

| Command | Description | Example |
|---------|-------------|---------|
| `dgit init` | Initialize repository | `dgit init` |
| `dgit scan` | Find design files | `dgit scan .` |
| `dgit add` | Stage files | `dgit add *.psd` |
| `dgit commit` | Create version | `dgit commit -m "message"` |
| `dgit status` | Check changes | `dgit status` |
| `dgit log` | View history | `dgit log` |
| `dgit restore` | Restore files | `dgit restore v2 file.psd` |

## 🧪 Testing

### Run the Test Suite
```bash
# Execute all tests
go test ./...

# Run with detailed output
go test -v ./...

# Test specific components
go test ./internal/commit/
go test ./internal/scanner/
```

### Performance Benchmarks
```bash
# Run performance tests
go test -bench=. ./internal/commit/
go test -bench=. ./internal/restore/
```

## 🤝 Contributing

We welcome contributions from the community!

### Getting Started
1. Fork the repository
2. Clone your fork: `git clone https://github.com/yourusername/dgit.git`
3. Create a feature branch: `git checkout -b new-feature`
4. Make your changes and add tests
5. Ensure tests pass: `go test ./...`
6. Commit and push your changes
7. Submit a pull request

### Development Guidelines
- Follow Go coding standards
- Add tests for new functionality
- Update documentation as needed
- Keep commits focused and descriptive

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

## 🏆 Project Recognition

- **University Competition 2025** - Innovation in Developer Tools
- **Technical Achievement** - Significant performance improvements for design workflows
- **User Experience** - Designer-focused interface design

## 📞 Contact & Support

- **Development Team**: 3px
- **Project Repository**: [GitHub](https://github.com/your-username/dgit)
- **Issues & Suggestions**: [GitHub Issues](https://github.com/your-username/dgit/issues)

## 🗺️ Development Roadmap

### Near Term (1-2 months)
- [ ] Enhanced Sketch and Figma file support
- [ ] Improved compression algorithms
- [ ] Performance optimizations

### Medium Term (3-6 months)
- [ ] Cross-platform GUI application
- [ ] Team collaboration features
- [ ] Plugin system for design tools

### Long Term (6+ months)
- [ ] Cloud synchronization service
- [ ] Advanced analytics and insights
- [ ] Enterprise features and support

---

<div align="center">

**DGit - Version Control, Designed for Designers** 

*Bringing intelligent version control to creative workflows*

</div>
