# Lodoxa Telegram Bot

## Overview
This is an Arabic-language Telegram bot for managing electronic charging services (games, apps, and payment services). The bot provides a comprehensive platform for users to purchase in-game currency, app subscriptions, and digital payment services with an integrated admin panel for management.

## Purpose
- Allow users to charge gaming accounts (PUBG Mobile, Free Fire, Steam, etc.)
- Manage app subscriptions and digital services
- Process payment services with multiple payment methods
- Admin panel for managing services, users, orders, and agents
- Agent system with commission tracking

## Current State
✅ **Running Successfully** - The bot is configured and running in Replit environment.

## Recent Changes
**October 27, 2025** (Latest Update)
- **🔧 Enhanced Referral System Display and Earnings Logic**: Improved user experience and earnings calculation
  - **Display Changes**: Referral page now displays for all users, regardless of purchase history
    - Users without purchases see "غير فعال 💤" (Inactive) status with clear note explaining activation requirement
    - Users with purchases see "فعال ⚡" (Active) status
    - Referral link and statistics always visible to encourage sharing
  - **Earnings Calculation**: Referral earnings now only count for users who have made at least one purchase
    - Level 1 referrers only receive earnings if they have `has_purchased = true`
    - Level 2 referrers only receive earnings if they have `has_purchased = true`
    - This prevents inactive users from accumulating earnings
  - **Withdrawal Button**: Always visible when user has earnings > 0, allowing transfer to main balance
  
- **✅ Completed Referral System Implementation**: Finalized the referral system to 100% functionality
  - Added `/referral_toggle` command for admins to enable/disable the referral system
  - Added `/referral_rates` command to adjust commission percentages (Level 1 and Level 2)
  - Fixed `handle_subscription_check` to display referral button for eligible users
  - Referral button now appears for users who have completed at least one purchase
  - Full integration with order processing to distribute referral earnings automatically
  - Two-level referral system: Level 1 (direct referrals) and Level 2 (indirect referrals)
  - Users can withdraw referral earnings to main balance
  - Badge system based on referral count (مسوّق مبتدئ, خبير تسويق, شريك ذهبي)
  - Unique referral link for each user with REF_ID format
  - Real-time notifications when referral earnings are added
  
- **Enhanced Admin Panel with Quick Statistics**: Admin control panel now displays key metrics at a glance
  - Shows pending orders count
  - Displays total users count
  - Shows total user balances
  - Calculates and displays user spending in last 15 days
  - Added `get_spending_last_15_days()` method in DataManager to calculate spending from completed orders
  - Fixed `get_pending_orders()` to correctly identify pending orders using "قيد المعالجة" status
  - Statistics displayed in clean, organized format with emoji icons

- **Enhanced Balance Recharge Messages**: Significantly improved all balance recharge flow messages
  - Main recharge menu now shows current balance and bot name
  - All payment methods (Syriatel Cash, Sham Cash, Payeer, USDT) now have clear step-by-step instructions
  - Added visual hierarchy with emoji icons (📋, 📮, 💱, ⚠️, etc.)
  - Improved error messages when payment methods are unavailable
  - Charge code input now has better guidance
  - USDT messages include BEP-20 network warning
  - All messages formatted consistently with Markdown for better readability
  - Exchange rates prominently displayed for USD-based methods
  
- **Improved Welcome Message UX**: Enhanced user experience for new users
  - Integrated "My Data" (بياناتي) button directly into welcome message
  - Removed redundant "Choose Service" (اختر خدمة) message
  - Cleaner interface with single welcome message containing both greeting and statistics button
  - Applied to both initial `/start` command and channel subscription verification
  - Removed "Payments 🌟" button from post-subscription menu (consistent with main menu)

**October 26, 2025**
- **Simplified Admin Management System**: Admin management now bot-only (no Telegram channel integration)
  - Removed automatic promotion/demotion of admins in Telegram channels
  - Admin privileges apply only to bot functionality and control panel access
  - Cleaner, simpler workflow with no channel permission requirements
  - Notification messages updated to reflect bot-only admin scope
  - All admin operations working smoothly without channel-related errors

**October 25, 2025**
- **Fixed ADMG01C Warning Message Feature**: Resolved issue where admin warning messages were not being sent
  - Added missing CallbackQueryHandler for warning confirmation buttons in ADMG01C_PANEL state
  - Warning messages now properly sent to all registered admins with detailed delivery report
  - Users can now successfully send subscription expiry warnings to all administrators
- Bot token and ADMG01C ID now configured directly in main.py (lines 32 and 38)
- Cleaned Python package cache and reinstalled python-telegram-bot cleanly
- Removed conflicting `telegram` package from requirements.txt
- Bot is running successfully and all features working correctly

**October 25, 2025** (Earlier)
- Migrated from GitHub to Replit environment
- Installed Python 3.11 and python-telegram-bot library v22.5
- Configured workflow to run the bot automatically
- Verified bot starts successfully and connects to Telegram API
- **Enhanced ADMG01C Panel**: 
  - Simplified bot name change feature - now accepts single name input (e.g., "Azzo Store") that automatically replaces all bot name occurrences throughout the application
  - **Admin Management System**: Comprehensive admin management within ADMG01C panel:
    - View all current admins with their details (name, user ID, creation date)
    - Add new admins by entering their User ID and name
    - Remove existing admins with confirmation
    - Automatic notifications sent to admins when added or removed
    - Admins stored securely in settings.json
  - **Warning Broadcast System**: Send subscription expiry warnings to all registered administrators

## Project Architecture

### Technology Stack
- **Language**: Python 3.11
- **Framework**: python-telegram-bot (v22.5)
- **Data Storage**: JSON files in `data/` directory
- **Bot Features**: Conversation handlers, inline keyboards, callback queries

### File Structure
```
.
├── main.py              # Main bot application (7,465 lines)
├── data/                # JSON data storage
│   ├── users.json      # User accounts and balances
│   ├── apps.json       # App charging services
│   ├── games.json      # Game charging services
│   ├── orders.json     # Order history
│   └── settings.json   # Bot settings and configuration
├── .gitignore          # Git ignore rules
├── pyproject.toml      # Python project configuration
└── replit.md           # This file
```

### Key Components

#### DataManager Class
Handles all data operations with JSON files:
- User management (balance, orders, freezing, banning)
- Apps/Games management
- Orders tracking
- Settings (support username, payment addresses)
- Charge codes generation and validation
- Payment services management
- Agent system management

#### LodoxaBot Class
Main bot logic with multiple conversation states:
- User menu navigation
- Service selection (apps, games, payments)
- Order processing
- Balance recharge (Syriatel Cash, Sham Cash, Payeer, USDT, charge codes)
- Admin panel (service management, user management, statistics)
- Agent panel (commission tracking, withdrawal requests)

### Admin Features
- Add/edit/delete apps, games, and payment services
- Manage categories and pricing (fixed or quantity-based)
- User management (ban, freeze, adjust balance)
- Order approval/rejection workflow
- Balance recharge request handling
- Broadcast messages to all users
- Agent management and commission tracking
- Statistics and analytics
- Bulk price adjustments

### User Features
- Browse and purchase app/game charging services
- Multiple payment methods for balance recharge
- View account statistics
- Support contact
- Order tracking with notifications

### Payment Methods
- Syriatel Cash
- Sham Cash (Syrian Pound)
- Payeer (USD with exchange rate)
- USDT BEP-20 (with exchange rate)
- Charge codes

## Configuration

### Environment Variables
- `TELEGRAM_BOT_TOKEN` (Required): Your Telegram bot token from @BotFather

### Bot Constants (in main.py)
- `ADMIN_ID`: Admin Telegram user ID (currently: 8319511583)
- `CHANNEL_USERNAME`: Required channel (@Lodoxa)
- `ORDERS_CHANNEL`: Channel ID for order notifications
- `BALANCE_REQUESTS_CHANNEL`: Channel ID for balance requests
- `NEW_USER_CHANNEL`: Channel ID for new user notifications

## How to Run

The bot runs automatically via the configured workflow. To manually restart:
1. Use the "Telegram Bot" workflow in the Replit interface
2. Or run: `python main.py`

The bot will:
1. Initialize data files if they don't exist
2. Load sample data for apps and games
3. Connect to Telegram API
4. Start polling for updates

## Data Storage

All data is stored in JSON files in the `data/` directory:
- **users.json**: User accounts with balance and order history
- **apps.json**: Available apps with categories and pricing
- **games.json**: Available games with categories and pricing
- **orders.json**: All orders with status and details
- **settings.json**: Bot configuration including:
  - Support username
  - Payment addresses (Syriatel, Sham Cash, Payeer, USDT)
  - Charge codes
  - Payment services
  - Agents and commission rates
  - Withdrawal fees

## Security Notes

⚠️ **Important**: 
- Bot token is stored securely in Replit Secrets (TELEGRAM_BOT_TOKEN)
- Never commit the bot token to git
- Admin ID is hardcoded - modify in main.py if needed
- All user data is stored locally in JSON files

## Development Notes

### Adding New Services
Services can be added through the admin panel via Telegram, or by directly editing the JSON files.

### Order Flow
1. User selects service and category
2. Enters account ID
3. Confirms order (balance is checked and deducted)
4. Order sent to admin channel for processing
5. Admin approves/rejects
6. User receives notification

### Agent System
- Agents earn commission on orders made through the bot
- Commission rates are configurable per agent
- Agents can request withdrawals (subject to fees)
- Agent earnings tracked separately from user balance

## Troubleshooting

### Bot Not Responding
1. Check workflow status in Replit
2. Verify TELEGRAM_BOT_TOKEN is set correctly
3. Check logs for errors

### Import Errors
- Ensure only `python-telegram-bot` is installed (not `telegram`)
- Run `uv sync` to reinstall dependencies if needed

### Data Issues
- Check `data/` directory exists and files are valid JSON
- Backup files are created automatically (.backup extension)

## Future Enhancements
- Database migration (PostgreSQL) for better scalability
- Webhook support instead of polling
- Multi-language support
- Analytics dashboard
- Automated order processing integration
