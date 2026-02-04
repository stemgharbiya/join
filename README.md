# STEM Gharbiya Github Join Portal

A modern, serverless application for students to apply for joining the STEM Gharbiya GitHub organization. Built with Cloudflare Pages, Workers, and D1 database.

## Features

- **Responsive Form**: Clean, accessible form with real-time validation
- **Dark/Light Theme**: Automatic theme detection with manual toggle
- **Duplicate Prevention**: Prevents multiple submissions with same email + GitHub username
- **Serverless Backend**: Cloudflare Workers for API endpoints
- **Database Storage**: Cloudflare D1 for persistent data storage
- **Email Notifications**: Automated emails to applicants and team via Resend
- **Error Handling**: Comprehensive client and server-side error management

## Tech Stack

- **Frontend**: HTML5, CSS3, Vanilla JavaScript
- **Backend**: Cloudflare Workers
- **Database**: Cloudflare D1 (SQLite)
- **Email**: Resend API
- **Deployment**: Cloudflare Pages
- **Development**: Wrangler CLI

## Prerequisites

- Node.js 18+
- Cloudflare account
- Resend account for email
- Git

## Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/stemgharbiya/join.git
   cd stemgharbiya-join
   ```

2. **Install dependencies**
   ```bash
   npm install
   # or
   pnpm install
   ```

3. **Set up Cloudflare D1 database**
   ```bash
   npx wrangler d1 create stemgharbiya-applications
   npx wrangler d1 execute stemgharbiya-applications --command="
     CREATE TABLE applications (
       id INTEGER PRIMARY KEY AUTOINCREMENT,
       fullName TEXT NOT NULL,
       schoolEmail TEXT NOT NULL,
       githubUsername TEXT NOT NULL,
       seniorYear TEXT NOT NULL,
       interests TEXT NOT NULL,
       motivation TEXT NOT NULL,
       timestamp TEXT NOT NULL
     );
   "
   ```

4. **Configure environment variables**

   Create `.dev.vars` file:
   ```
   RESEND_API_KEY=your_resend_api_key
   RESEND_SENDER_EMAIL=your-verified-email@resend.dev
   TEAM_NOTIFICATION_EMAIL=team@example.com
   ```

5. **Start development server**
   ```bash
   npm run dev
   # or
   pnpm dev
   ```

   Open [http://localhost:8788](http://localhost:8788)

## Email Configuration

1. Sign up for [Resend](https://resend.com)
2. Verify your sender domain/email
3. Get your API key
4. Update environment variables

## Deployment

1. **Update wrangler.toml**
   - Replace placeholder database ID with your actual D1 database ID
   - Update environment variables in Cloudflare Dashboard

2. **Deploy to Cloudflare Pages**
   ```bash
   npx wrangler pages deploy .
   ```

3. **Set production environment variables**
   ```bash
   npx wrangler secret put RESEND_API_KEY
   npx wrangler secret put RESEND_SENDER_EMAIL
   npx wrangler secret put TEAM_NOTIFICATION_EMAIL
   ```

## Database Management

### View submissions (local)
```bash
npx wrangler d1 execute stemgharbiya-applications --command="SELECT * FROM applications;"
```

### View submissions (production)
```bash
npx wrangler d1 execute stemgharbiya-applications --remote --command="SELECT * FROM applications;"
```

### Backup database
```bash
npx wrangler d1 backup create stemgharbiya-applications --name backup-$(date +%Y%m%d)
```

## Usage

1. Fill out the application form with:
   - Full name
   - School email (@stemgharbiya.moe.edu.eg)
   - GitHub username
   - Senior year (S25-S30)
   - Interests (select at least one)
   - Motivation (10-500 characters)

2. Submit the form
3. Receive confirmation email
4. Team gets notification email

## Development

### Project Structure
```
├── join/
│   └── index.html          # Main application form
├── functions/
│   └── api/
│       └── submit.js       # Cloudflare Worker API
├── wrangler.toml           # Cloudflare configuration
├── package.json
└── README.md
```

### Available Scripts
- `npm run dev` - Start development server
- `npm run deploy` - Deploy to production

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Validation Rules

- **Email**: Must end with `@stemgharbiya.moe.edu.eg`
- **Senior Year**: S25, S26, S27, S28, S29, or S30 only
- **Interests**: At least one must be selected
- **Motivation**: 10-500 characters
- **Duplicates**: Same email + GitHub username combination blocked

## Security

- Input sanitization and validation
- SQL injection prevention via prepared statements
- XSS protection via HTML escaping in emails
- Environment variables for sensitive data

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Built for STEM Gharbiya community
- Powered by Cloudflare's edge platform
- Email service by Resend