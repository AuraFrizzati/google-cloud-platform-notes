# GitHub Pages Template

Template ready-to-be-used to publish web-accessible content via GitHub Pages deployed using GitHub Actions

## 🚀 Setting Up GitHub Pages

To deploy this website on GitHub Pages:

1. **Push your code to GitHub:**
   ```bash
   git add .
   git commit -m "Initial GitHub pages website"
   git push origin main
   ```

2. **Enable GitHub Pages:**
   - Go to your repository on GitHub
   - Click on **Settings**
   - Scroll down to **Pages** section (in the left sidebar)
   - Under **Source**, select **main** branch
   - Keep the folder as **/ (root)**
   - Click **Save**

3. **Wait for deployment:**
   - GitHub will automatically build and deploy your site
   - This usually takes 1-2 minutes
   - You'll see a green checkmark when it's ready

4. **Access your site:**
   - Your site will be available at: `https://github.com/AuraFrizzati/github-pages-template/`
   - You can find this URL in the Pages settings

## 🛠️ Local Development

To view the website locally:

1. Simply open `index.html` in your web browser, or
2. Use a local server:
   ```bash
   # Using Python 3
   python3 -m http.server 8000
   
   # Or using Python 2
   python -m SimpleHTTPServer 8000
   ```
   Then visit `http://localhost:8000`
s

## 🎨 Customization

Feel free to customize the website:

- **Colors**: Edit CSS variables in `styles.css` (`:root` section)
- **Content**: Update sections in `index.html`
- **Layout**: Modify grid layouts and responsive breakpoints in `styles.css`

## 📝 License

This project is open source and available for educational purposes.

## 🤝 Contributing

Suggestions and improvements are welcome! Feel free to open an issue or submit a pull request.
