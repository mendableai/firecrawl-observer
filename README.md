# Firecrawl Observer

<p align="center">
  <img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExYm94OHU0a3MxM2d4Zm5zb2tseTF4bDdmZWZpcmFlaDYxMzZod3VmbCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/pBQeHEiQiR25n8rd9J/giphy.gif" alt="Firecrawl Observer Demo" width="100%" />
</p>

A powerful website monitoring application that tracks changes on websites and sends intelligent notifications. Built with Next.js, Convex, and Firecrawl API.

## Features

- **Website Monitoring**: Track changes on single pages or entire websites
- **AI-Powered Analysis**: Filter out noise with intelligent change detection
- **Flexible Notifications**: Email, webhooks, or dashboard-only monitoring
- **Secure API Key Management**: Encrypted storage for Firecrawl and AI provider keys
- **Real-time Updates**: Instant change detection with configurable intervals
- **Beautiful UI**: Modern, responsive interface with dark mode support

## Prerequisites

- Node.js 18+ and npm
- A [Convex](https://convex.dev) account (free tier available)
- A [Firecrawl](https://firecrawl.dev) API key (required for website monitoring)
- (Optional) [Resend](https://resend.com) API key for email notifications
- (Optional) AI API key for intelligent filtering ([OpenAI](https://platform.openai.com), [Anthropic](https://console.anthropic.com), [Google](https://aistudio.google.com), or [Moonshot](https://platform.moonshot.cn))

## Quick Start

### Step 1: Clone the Repository

```bash
git clone https://github.com/mendableai/firecrawl-observer.git
cd firecrawl-observer
```

### Step 2: Install Dependencies

```bash
npm install
```

### Step 3: Initialize Convex

```bash
npx convex dev
```

This will:
- Open your browser to authenticate with Convex
- Create a new project (or link to an existing one)
- Set up your database schema

Keep this terminal running.

### Step 4: Configure Authentication (in a new terminal)

```bash
npx @convex-dev/auth
```

This will:
- Set up your SITE_URL (use http://localhost:3000 for local development)
- Generate and configure JWT keys automatically
- Configure all necessary auth files

When prompted for the local web server URL, enter: `http://localhost:3000`

### Step 5: Set Additional Environment Variables

```bash
# Generate and set encryption key for API key storage
npx convex env set ENCRYPTION_KEY "$(openssl rand -base64 32)"

# Optional: Set up email notifications
npx convex env set RESEND_API_KEY "your_resend_api_key"
npx convex env set FROM_EMAIL "noreply@yourdomain.com"
```

### Step 6: Start the Development Server

```bash
npm run dev
```

Visit [http://localhost:3000](http://localhost:3000) to see the application.

## Configuration

### Environment Variables

Create a `.env.local` file with:

```env
# Convex (automatically set by npx convex dev)
NEXT_PUBLIC_CONVEX_URL=https://your-project.convex.cloud

# Email notifications (optional)
RESEND_API_KEY=re_YOUR_RESEND_API_KEY

# Encryption key (required - auto-generated by setup script)
ENCRYPTION_KEY=your_generated_encryption_key

# Auth configuration
SITE_URL=http://localhost:3000
JWT_PRIVATE_KEY=your_jwt_private_key
```

### Convex Environment Variables

Required variables in Convex dashboard:

- `JWT_PRIVATE_KEY` - Automatically set by `npx @convex-dev/auth` (required for auth)
- `JWKS` - Automatically set by `npx @convex-dev/auth` (required for auth)
- `SITE_URL` - Automatically set by `npx @convex-dev/auth` (required)
- `ENCRYPTION_KEY` - For securing API keys in database (required)
- `FIRECRAWL_API_KEY` - For website monitoring (optional, users can add their own)
- `RESEND_API_KEY` - For email notifications (optional)
- `FROM_EMAIL` - Sender email address (optional)

## Usage

### Adding a Website to Monitor

1. Sign up or log in to your account
2. Click "Add Website"
3. Choose monitoring type:
   - **Single Page**: Monitor a specific URL
   - **Full Site**: Crawl and monitor an entire website
4. Configure check interval (minimum 60 minutes)
5. Set up notifications (optional)

### API Key Management

Users can add their own API keys for:
- **Firecrawl**: Required for website monitoring
- **AI Provider**: Optional, for intelligent change filtering

API keys are encrypted before storage using AES-256-GCM encryption.

### Notification Options

1. **Email Notifications**
   - Requires email verification
   - Powered by Resend
   - Includes change summaries and AI analysis

2. **Webhook Notifications**
   - Send changes to your own endpoints
   - Configurable payload format
   - Includes full diff and metadata

3. **AI-Filtered Notifications**
   - Only receive notifications for meaningful changes
   - Configurable threshold (0-100)
   - Includes AI reasoning for each change

## Development

### Project Structure

```
fc-observer/
├── convex/              # Backend functions and schema
│   ├── auth.ts          # Authentication logic
│   ├── schema.ts        # Database schema
│   ├── firecrawl.ts     # Firecrawl integration
│   └── aiAnalysis.ts    # AI change analysis
├── src/
│   ├── app/            # Next.js app router pages
│   ├── components/     # React components
│   ├── config/         # App configuration
│   └── lib/            # Utilities and helpers
└── scripts/            # Test scripts (optional)
```

### Available Scripts

- `npm run dev` - Start development servers
- `npm run build` - Build for production
- `npm run lint` - Run ESLint
- `npx convex dev` - Start Convex backend
- `npx convex deploy` - Deploy to production
- `npx @convex-dev/auth` - Configure authentication

### Testing

Before committing, ensure your code passes:

```bash
npm run lint
npm run typecheck
```

## Deployment

### Deploy to Vercel (Recommended)

1. Push your code to GitHub
2. Import project in Vercel
3. Add environment variables:
   - `NEXT_PUBLIC_CONVEX_URL` (from Convex dashboard)
   - `RESEND_API_KEY` (if using email)
4. Deploy

### Deploy Convex Backend

```bash
npx convex deploy
```

Update your production environment variables in Convex dashboard.

## Security

- All API keys are encrypted using AES-256-GCM
- JWT-based authentication with secure sessions
- Email verification required for notifications
- Rate limiting on API endpoints
- Secure webhook URL validation

## Troubleshooting

### Common Issues

1. **"Encryption key not set" error**
   - Ensure `ENCRYPTION_KEY` is set in both `.env.local` and Convex environment

2. **Authentication errors**
   - Regenerate JWT keys using `node scripts/generate-jwt-keys.js`
   - Ensure `SITE_URL` matches your domain

3. **Firecrawl API errors**
   - Verify API key is valid
   - Check API rate limits
   - Ensure URLs are accessible

### Debug Mode

View backend logs:

```bash
npx convex logs
```

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

- [Report Issues](https://github.com/mendableai/firecrawl-observer/issues)
- [Firecrawl Documentation](https://docs.firecrawl.dev)
- [Convex Documentation](https://docs.convex.dev)