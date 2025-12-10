# ATOM.GAME Internal Management Panel - Design Guidelines

## Design Approach

**Selected System**: Material Design + Linear-inspired productivity patterns

**Justification**: This is a data-heavy internal productivity tool requiring clear information hierarchy, efficient workflows, and robust data visualization. The combination provides:
- Strong grid systems for complex data layouts
- Clear component patterns for forms and tables
- Professional aesthetic suitable for B2B tools
- Excellent patterns for multi-step workflows and approval states

**Key Design Principles**:
1. **Clarity Over Decoration** - Every element serves a functional purpose
2. **Scannable Hierarchy** - Users must quickly locate critical information (budget alerts, pending approvals, deadlines)
3. **Workflow Visibility** - Event status and approval states are always clear
4. **Role-Based Layouts** - Each user role sees optimized views for their tasks

---

## Typography

**Font Families**:
- **Primary**: Inter (UI text, data tables, forms)
- **Headings**: Inter SemiBold/Bold
- **Monospace**: JetBrains Mono (budget numbers, dates, IDs)

**Type Scale**:
- Page Titles: text-3xl font-bold (super_admin dashboards)
- Section Headers: text-xl font-semibold
- Card Titles: text-lg font-semibold
- Body Text: text-base
- Table Data: text-sm
- Labels/Meta: text-xs font-medium uppercase tracking-wide
- Budget Numbers: text-lg font-mono font-semibold

---

## Layout System

**Spacing Primitives**: Use Tailwind units of **2, 4, 6, 8, 12, 16**

- Component padding: p-4 to p-6
- Section spacing: space-y-6 to space-y-8
- Card gaps: gap-4
- Dashboard grid gaps: gap-6
- Page margins: p-6 to p-8

**Grid Patterns**:
- Super_admin dashboard: 3-column stats grid (grid-cols-3) + full-width tables
- Event detail pages: 2-column layout (2/3 main content, 1/3 sidebar for status/actions)
- Budget breakdown: Single column with nested 2-column line items
- City list: Grid cards (grid-cols-1 md:grid-cols-2 lg:grid-cols-3)

---

## Component Library

### Navigation
**Sidebar Navigation** (All Roles):
- Fixed left sidebar (w-64)
- Logo at top
- Role-appropriate menu items with icons
- Active state with border-l-4 indicator
- User profile/role badge at bottom

**Top Bar**:
- Breadcrumb navigation for deep pages
- Global search (for events, cities)
- Notification bell (pending approvals, deadlines)
- User dropdown menu

### Data Display

**Status Badges**:
- Pill-shaped badges (px-3 py-1 rounded-full text-xs font-medium)
- Event workflow states: calendar_draft, calendar_approved, budget_draft, budget_approved
- Budget status: within_limit, approaching_limit, exceeded
- Deadline status: upcoming, due_soon, overdue

**Data Tables**:
- Striped rows (alternate row treatment)
- Sticky headers for long tables
- Sortable columns with arrow indicators
- Row actions menu (three-dot dropdown)
- Empty states with clear CTAs

**Cards**:
- Elevated cards with subtle shadow (shadow-sm hover:shadow-md)
- Standard padding (p-6)
- Card header with title + action button
- Clear visual separation between sections

**Progress Indicators**:
- Budget utilization: Horizontal progress bar with percentage label
- Event preparation checklist: Vertical checklist with checkboxes
- City budget overview: Stacked progress bars showing allocated vs remaining

### Forms & Inputs

**Form Layouts**:
- Label above input (mb-2)
- Helper text below input (text-sm text-muted mt-1)
- Error states with border treatment and error message
- Required field indicators (asterisk)

**Input Fields**:
- Consistent height (h-10 for text inputs)
- Clear border treatment
- Focus states with border emphasis
- Disabled state clearly visible

**Multi-Step Forms** (Event Creation):
- Step indicator at top (numbered circles with connecting lines)
- Current step highlighted
- Previous steps shown as completed (checkmark)
- "Save Draft" and "Submit for Approval" button hierarchy

### Action Buttons

**Button Hierarchy**:
- Primary actions: Solid fill (e.g., "Approve Budget", "Create Event")
- Secondary actions: Outlined (e.g., "Save Draft", "Cancel")
- Destructive: Solid fill distinct treatment (e.g., "Block Organizer")
- Button sizes: h-10 (default), h-12 (prominent CTAs)

### Dashboard Widgets

**Stat Cards** (Super_admin Dashboard):
- 3-column grid
- Large number display (text-3xl font-bold)
- Label below (text-sm)
- Trend indicator (arrow icon + percentage)
- Clickable cards link to filtered views

**City Budget Cards**:
- City name as header
- Budget progress bar (visual representation)
- Three-line summary: Total budget / Allocated / Remaining
- Quick actions dropdown

**Pending Approvals Queue**:
- List of cards showing event name, city, type (calendar/budget), submitted date
- "Review" button for each item
- Badge count on navigation menu item

**City Rankings Table**:
- Sortable columns: Events count, participants, social engagement, budget efficiency
- Ranking number in first column
- City name with flag/icon
- Metrics in data columns
- Drill-down link to city detail

### Event Detail Pages

**Tab Navigation**:
- Horizontal tabs (Calendar | Budget | Checklist | Report)
- Active tab underlined (border-b-2)
- Tab content with consistent padding (p-6)

**Calendar Tab**:
- Event details in 2-column grid (label: value pairs)
- Edit button (if permissions allow)
- Status history timeline showing approval flow

**Budget Tab**:
- Category headers (Prizes, Venue, Equipment, etc.)
- Line items in each category with amount
- Subtotals per category
- Grand total prominently displayed (text-2xl font-bold)
- Approval status and notes from super_admin

**Checklist Tab**:
- Vertical list of tasks
- Checkbox for each item
- Assignee (city_org or smm)
- Due date with visual warning if overdue
- Completion percentage at top

**Report Tab** (Post-Event):
- Form layout with sections: Participation, Winners, Social Stats, Media
- Input fields for metrics
- Media upload area (drag-and-drop zone)
- Links list (social posts, press mentions)
- Submit report button

### SMM Interface

**Event Feed**:
- Card-based layout (similar to social media feed)
- Event card shows: Name, date, city, key stats, media count
- Filter options: Upcoming, Past, Needs Announcement, Needs Report
- Quick action buttons: Upload Media, Mark Task Complete

**Media Upload Modal**:
- Drag-and-drop file upload zone
- Preview thumbnails
- Tag media by type (photo, video, clip, stream recording)
- Link to event automatically

---

## Accessibility & Interaction

- All interactive elements have min-height h-10 (touch targets)
- Form inputs have associated labels
- Tables have proper header markup
- Status changes show toast notifications
- Loading states for data-heavy operations (skeleton loaders)
- Keyboard navigation supported throughout

---

## Images

**No hero images** - This is an internal tool focused on functionality.

**Icon Usage**:
- Use **Heroicons** via CDN for all interface icons
- Icons in navigation menu (outline variant)
- Icons in status badges and buttons (solid variant, 16px)
- Icons for empty states (48px, centered)

**Avatar/Logo**:
- ATOM.GAME logo in sidebar (square, 40x40)
- User avatars in top bar (circular, 32x32)
- City flags/icons in lists (optional, 24x24)

---

## Animations

**Minimal, Purposeful Only**:
- Page transitions: None (instant navigation)
- Dropdown menus: Fade in (duration-200)
- Toast notifications: Slide in from top-right (duration-300)
- Data loading: Skeleton pulse animation
- NO scroll-triggered animations
- NO decorative animations