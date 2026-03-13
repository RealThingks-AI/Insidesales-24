

## Plan: Build User Dashboard with Dashboard/Revenue Toggle

### Overview
Rewrite the Dashboard page to have two toggle views: **Dashboard** (widget-based overview) and **Revenue** (existing YearlyRevenueSummary). The Dashboard view will display 8 widgets in a grid layout matching the reference images.

### Widgets (8 total)
| Widget | Table | Filter | Display |
|--------|-------|--------|---------|
| My Deals | `deals` | `lead_owner = user.id` | Counts by stage (RFQ, Offered, Won, Lost) |
| My Accounts | `accounts` | `account_owner = user.id` | Counts by status (New, Working, Hot, Nurture) |
| My Contacts | `contacts` | `contact_owner = user.id` | Counts by source (Website, Referral, LinkedIn, Other) |
| Action Items | `action_items` | `assigned_to = user.id` | Counts by status (Open, In Progress, Completed, Cancelled) |
| Email Statistics | `campaign_communications` | `created_by = user.id`, type=email | Sent, Opened, Open Rate |
| Quick Actions | Static | — | Buttons: + Contact, + Account, + Deal, + Task |
| Today's Agenda | `action_items` | due today, `assigned_to = user.id` | List of today's tasks |
| Recent Activities | `security_audit_log` | recent CRUD ops | Activity feed with badges |

### Files to Create/Modify

**Modify: `src/pages/Dashboard.tsx`**
- Add toggle state (`'dashboard' | 'revenue'`)
- Header: greeting with user name, toggle buttons, NotificationBell, year selector (revenue mode only)
- Render `UserDashboard` or `YearlyRevenueSummary` based on toggle

**Create: `src/hooks/useDashboardData.tsx`**
- Single hook fetching all widget data in parallel via `Promise.all`
- All queries filtered by `user.id`
- Uses React Query with `['dashboard-data']` key

**Create: `src/components/dashboard/UserDashboard.tsx`**
- Grid layout: 4-col top rows, 2-col bottom row
- Renders all 8 widget components

**Create 8 widget components in `src/components/dashboard/widgets/`:**
- `MyDealsWidget.tsx` — stage counts with colored numbers, "View All" link to /deals
- `MyAccountsWidget.tsx` — status counts, "+ Add Account" button
- `MyContactsWidget.tsx` — source counts, "+ Add Contact" button
- `ActionItemsWidget.tsx` — status counts, "+ Add Task" button
- `EmailStatsWidget.tsx` — sent/opened/rate display
- `QuickActionsWidget.tsx` — action buttons opening modals or navigating
- `TodaysAgendaWidget.tsx` — today's tasks list with "Clear day ahead" empty state
- `RecentActivitiesWidget.tsx` — audit log feed with module name, action badge, relative time

### UI Design
- Cards with border, rounded corners, consistent with existing `Card` component
- Large colored numbers for counts (green for positive, red for lost/cancelled, blue for active)
- "+ Add" buttons in card headers
- Bottom section: 2-column layout for Today's Agenda (left) and Recent Activities (right)
- Toggle buttons styled as pill/segment control in the header

