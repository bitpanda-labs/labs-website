# Bitpanda Labs Website

🚀 **The official website for Bitpanda Labs** - Bitpanda's innovation playground for internal innovation, tech experiments, and product prototypes.

[![Live Site](https://img.shields.io/badge/Live-bitpanda--labs.github.io%2Flabs--website-blue)](https://bitpanda-labs.github.io/labs-website/)
[![Jekyll](https://img.shields.io/badge/Built%20with-Jekyll-red)](https://jekyllrb.com/)
[![GitHub Pages](https://img.shields.io/badge/Deployed%20on-GitHub%20Pages-green)](https://pages.github.com/)

## 🌟 About

Bitpanda Labs showcases cutting-edge fintech experiments, research prototypes, and innovative solutions that shape the future of digital finance. This website serves as a public window into our innovation process and a platform for the community to explore our latest experiments.

## 🚀 Quick Start

### Prerequisites
- Ruby 3.x
- Jekyll
- Bundler

### Local Development

1. **Clone the repository:**
   ```bash
   git clone git@github.com:bitpanda-labs/labs-website.git
   cd labs-website
   ```

2. **Install dependencies:**
   ```bash
   sudo gem install jekyll bundler  -n /usr/local/bin
   bundle install
   ```

3. **Run the development server:**
   ```bash
   bundle exec jekyll serve
   ```

4. **View the site:**
   Open http://localhost:4000 in your browser

### Using Dev Container (Recommended)

This project includes a VS Code dev container for consistent development environments:

1. **Open in VS Code**
2. **Install the "Remote - Containers" extension**
3. **Click "Reopen in Container" when prompted**
4. **The container will automatically install Jekyll and dependencies**

## 📁 Project Structure

```
├── _experiments/          # Experiment posts and templates
│   ├── README.md          # Contribution guidelines
│   └── _template.md       # Experiment template
├── _layouts/              # Jekyll page layouts
│   └── default.html       # Main layout template
├── _config.yml            # Jekyll configuration
├── index.html             # Homepage
└── .devcontainer/         # Dev container configuration
```

## 🧪 Contributing Experiments

We welcome contributions from the community! To add your experiment:

1. **Read the guidelines:** Check `_experiments/README.md`
2. **Use the template:** Copy `_experiments/_template.md`
3. **Create your experiment:** Fill in the template with your details
4. **Submit a pull request:** We'll review and merge approved experiments

### Experiment Categories
- AI & Machine Learning
- User Experience  
- Blockchain & DeFi
- Payment Innovation
- Security & Privacy
- Data Analytics
- Mobile Development
- API Development

## 🎨 Customization

### Adding New Layouts
Create new layout files in the `_layouts/` directory following Jekyll conventions.

### Styling
The main styles are included in `_layouts/default.html`. For larger projects, consider extracting CSS to separate files.

### Configuration
Modify `_config.yml` to:
- Update site metadata
- Add new collections
- Configure build settings

## 🚀 Deployment

This site is automatically deployed to GitHub Pages when changes are pushed to the `main` branch.

**Live URL:** https://bitpanda-labs.github.io/labs-website/

### Manual Deployment
```bash
bundle exec jekyll build
# Upload _site/ contents to your hosting provider
```

## 🛠️ Technical Details

- **Built with:** Jekyll static site generator
- **Hosting:** GitHub Pages
- **Dependencies:** Ruby, Jekyll, Bundler
- **Dev Environment:** VS Code dev container with Ruby 3.x

## 📋 Development Workflow

1. **Create feature branch:** `git checkout -b feature/your-feature-name`
2. **Make changes** and test locally
3. **Commit changes:** Follow conventional commit format
4. **Push and create PR:** Submit for review
5. **Merge to main:** Automatically deploys to live site

## 🤝 Contributing

We welcome contributions! Please:

1. **Fork the repository**
2. **Create a feature branch**
3. **Follow our coding standards:**
   - Use proper Jekyll/Liquid syntax
   - Maintain consistent formatting
   - Test locally before submitting
4. **Submit a pull request**

## 📝 License

This project is open source. Please check with Bitpanda Labs for specific licensing terms.

## 🔗 Links

- **Live Website:** https://bitpanda-labs.github.io/labs-website/
- **Bitpanda:** https://www.bitpanda.com/
- **Jekyll Documentation:** https://jekyllrb.com/docs/

## 💬 Support

For questions or support:
- **Open an issue** in this repository
- **Contact Bitpanda Labs** through official channels

---

**Built with ❤️ by the Bitpanda Labs team** 
