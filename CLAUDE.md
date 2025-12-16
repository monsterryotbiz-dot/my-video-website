# CLAUDE.md - AI Assistant Guide for PlayMon Video Website

## Project Overview

**PlayMon** is a Korean-language webtoon AI animation platform - a single-page web application for browsing and watching video content. The project is intentionally simple, using a single HTML file with embedded React, CSS, and JavaScript.

**Project Type:** Video gallery/streaming platform prototype
**Language:** Korean (ko)
**Tech Stack:** React 18 (CDN), Babel Standalone, Vanilla CSS, HTML5 Video
**Deployment:** Static hosting (no build step required)

---

## Repository Structure

```
my-video-website/
├── index.html          # Main application file (contains ALL code)
├── README.md           # Brief project description
└── CLAUDE.md           # This file
```

### Critical Architecture Decision

**Everything is in one file.** The entire application exists in `index.html`:
- HTML structure
- Inline CSS styles (lines 10-412)
- React application code (lines 417-638)
- No separate JavaScript, CSS, or asset files
- No build process, bundler, or package.json

---

## Technology Stack Details

### Frontend Framework
- **React 18.x** - Loaded via unpkg CDN
- **React DOM** - For rendering
- **Babel Standalone** - For JSX transformation in the browser
- Uses React Hooks (`useState`, `useEffect`)

### Styling
- **Vanilla CSS** - No preprocessors (Sass, Less, etc.)
- **Inline styles** in `<style>` tag (lines 10-412)
- **Gradient backgrounds** for content thumbnails
- **Responsive design** with mobile breakpoint at 768px
- **Sticky headers** using `position: sticky`

### Video Playback
- **HTML5 `<video>` element** with native controls
- **Sample videos** from Google Cloud Storage (BigBuckBunny, ElephantsDream, etc.)
- **Auto-play** enabled on episode selection
- Videos are external URLs, not locally hosted

### State Management
- **Local React state** using `useState` hooks
- **No Redux, Context API, or external state library**
- State structure:
  - `currentPage`: 'home' | 'detail' | 'player'
  - `selectedContent`: Currently selected webtoon object
  - `showAdmin`: Boolean for admin modal visibility
  - `contentList`: Array of content items
  - `editingId`: ID of item being edited in admin panel

---

## Application Architecture

### Page Navigation System

The app uses **client-side routing** via state (`currentPage`):

1. **Home Page** (`currentPage === 'home'`)
   - Hero banner with featured content
   - Grid of content cards
   - Admin button in header

2. **Detail Page** (`currentPage === 'detail'`)
   - Banner with content title and rating
   - List of episodes
   - Back button to home

3. **Player Page** (`currentPage === 'player'`)
   - Video player with controls
   - Episode information
   - Next episode button
   - Back button to detail

4. **Admin Modal** (overlay, not a page)
   - Content management interface
   - Edit and delete functionality
   - Overlays on top of home page

### Data Structure

Content items follow this schema:

```javascript
{
  id: number,              // Unique identifier
  title: string,           // Korean title (e.g., '전지적 독자 시점')
  rating: number,          // Decimal rating (9.5-9.9)
  status: string,          // '최신화' | '완결' | '시청중' | '무료'
  gradient: string,        // CSS linear-gradient for thumbnail
  episodes: [
    {
      ep: number,          // Episode number
      title: string,       // Episode title in Korean
      duration: string,    // Display duration (e.g., '1:34')
      videoUrl: string     // Full URL to video file
    }
  ]
}
```

### Initial Content

The app ships with 4 pre-loaded webtoons (lines 427-469):
1. 전지적 독자 시점 (Omniscient Reader's Viewpoint) - 2 episodes
2. 나 혼자만 레벨업 (Solo Leveling) - 1 episode
3. 화산귀환 (Return of the Mount Hua Sect) - 1 episode
4. 괴력 난신 (Strange Power) - 1 episode

---

## Key Components and Their Locations

| Component | Lines | Purpose |
|-----------|-------|---------|
| App | 420-636 | Root component with all state and routing |
| HomePage | 475-513 | Main landing page with content grid |
| DetailPage | 515-554 | Individual content detail and episode list |
| PlayerPage | 556-593 | Video player interface |
| AdminModal | 595-625 | Content management overlay |

---

## Development Workflows

### Making Changes to the UI

1. **Locate the relevant section** in index.html
2. **Edit inline CSS** (lines 10-412) for styling changes
3. **Edit JSX components** (lines 417-638) for functionality changes
4. **Test in browser** - Simply refresh the page (no build step)

### Adding New Content

To add a new webtoon, modify the `initialContent` array (lines 427-469):

```javascript
{
  id: 5,  // Increment ID
  title: 'New Webtoon Title',
  rating: 9.6,
  status: '최신화',
  gradient: 'linear-gradient(135deg, #color1 0%, #color2 100%)',
  episodes: [
    {
      ep: 1,
      title: 'Episode Title',
      duration: 'X:XX',
      videoUrl: 'https://path-to-video.mp4'
    }
  ]
}
```

### Modifying Styles

**CSS Classes are organized by section:**
- **Layout:** .page, .header, .content-section
- **Navigation:** .header-content, .logo, .admin-btn, .back-btn
- **Content Display:** .content-grid, .content-card, .thumbnail
- **Badges:** .rating-badge, .status-badge
- **Player:** .video-player, .player-page, .player-header
- **Admin:** .admin-overlay, .admin-modal, .admin-item

### Testing Video URLs

Current videos use Google Cloud Storage samples:
- `BigBuckBunny.mp4` - 1:34 duration
- `ElephantsDream.mp4` - 0:52 duration
- `ForBiggerBlazes.mp4` - 0:15 duration
- `Sintel.mp4` - 0:52 duration

To add new videos, use publicly accessible MP4 URLs.

---

## Code Conventions

### Naming Conventions

- **Components:** PascalCase (HomePage, DetailPage, PlayerPage)
- **Functions:** camelCase (setCurrentPage, setShowAdmin)
- **CSS Classes:** kebab-case (content-grid, admin-btn, rating-badge)
- **State Variables:** camelCase (currentPage, selectedContent, showAdmin)

### React Patterns Used

1. **Functional Components** - No class components
2. **Hooks** - useState, useEffect
3. **Conditional Rendering** - Using && and ternary operators
4. **Event Handlers** - Inline arrow functions in JSX
5. **Props Passing** - Implicit via closures (no explicit props)
6. **Key Props** - Using content.id and ep.ep

### Korean Language Content

All user-facing text is in Korean:
- UI labels: "관리자" (Admin), "지금 시청하기" (Watch Now)
- Status badges: "최신화" (Latest), "완결" (Complete), "시청중" (Watching)
- Action buttons: "수정" (Edit), "삭제" (Delete)

When adding new text, maintain Korean language consistency.

---

## Common Tasks for AI Assistants

### Task 1: Add a New Content Item

**Steps:**
1. Read index.html (lines 427-469)
2. Add new object to `initialContent` array
3. Ensure unique ID
4. Provide Korean title and episode titles
5. Use valid gradient CSS
6. Include at least one episode with valid video URL

### Task 2: Modify Styling

**Steps:**
1. Identify the CSS class (lines 10-412)
2. Edit the relevant style properties
3. Check responsive breakpoint at line 406 if mobile is affected
4. Test in browser

### Task 3: Add New Page/View

**Steps:**
1. Add new state for page tracking if needed
2. Create new component function (follow HomePage/DetailPage pattern)
3. Update conditional rendering logic (lines 627-635)
4. Add navigation buttons with setCurrentPage calls
5. Update CSS with new classes

### Task 4: Implement Admin Features

**Current admin functionality:**
- View all content (lines 603-621)
- Delete content with confirmation
- Edit button exists but not implemented

**To implement edit:**
1. Add editing state management
2. Replace content display with form inputs when editing
3. Add save/cancel handlers
4. Update contentList state on save

### Task 5: Add Data Persistence

**Current state:** All data resets on page reload

**To add persistence:**
1. Use `localStorage.setItem()` in useEffect when contentList changes
2. Use `localStorage.getItem()` on initial load
3. Parse JSON from localStorage or fall back to initialContent
4. Consider adding export/import functionality

---

## Important Constraints

### What NOT to Do

1. **Don't split into multiple files** - This is a single-file architecture by design
2. **Don't add a build system** - The app runs directly in the browser
3. **Don't add npm/package.json** - All dependencies are CDN-based
4. **Don't use external CSS frameworks** - Keep inline CSS only
5. **Don't add server-side code** - This is a static client-only app
6. **Don't change language to English** - Maintain Korean UI text

### Performance Considerations

- **Inline Babel transformation** is slow - Not suitable for production at scale
- **No code splitting** - Entire app loads at once
- **No image optimization** - Uses gradients instead of images
- **External video hosting** - Videos are not bundled

### Browser Compatibility

- Requires modern browser with ES6+ support
- Uses CSS Grid (IE11 not supported)
- Uses aspect-ratio CSS property (check caniuse)
- HTML5 video element required

---

## Git Workflow

### Current Branch
- Working branch: `claude/claude-md-mj7zuwqh27dwqarz-t67hI`
- Main branch: Not specified (check repository settings)

### Committing Changes

When making changes:
1. Use descriptive commit messages in English
2. Commit related changes together
3. Use format: `<type>: <description>`
   - feat: New feature
   - fix: Bug fix
   - style: CSS/styling changes
   - refactor: Code restructuring
   - docs: Documentation updates

### Pushing to Remote

Always push to the feature branch:
```bash
git push -u origin claude/claude-md-mj7zuwqh27dwqarz-t67hI
```

---

## Debugging Tips

### Common Issues

1. **Blank screen** - Check browser console for JSX syntax errors
2. **Styles not applying** - Verify CSS class names match (case-sensitive)
3. **Video not playing** - Check video URL accessibility and CORS
4. **React not defined** - Ensure CDN scripts load before Babel script
5. **Navigation broken** - Verify currentPage state values

### Browser Console

Open DevTools and check:
- **Console tab** - For JavaScript errors
- **Network tab** - For CDN script loading
- **Elements tab** - For rendered HTML inspection

### Testing Videos

Test video URLs manually:
```javascript
// In browser console
let video = document.createElement('video');
video.src = 'YOUR_VIDEO_URL';
video.play();
```

---

## Future Enhancement Ideas

Potential improvements (not implemented):

1. **Data Persistence** - localStorage or backend API
2. **Search Functionality** - Filter content by title
3. **User Authentication** - Protect admin panel
4. **Watchlist/Favorites** - User-specific content tracking
5. **Comments System** - Episode discussion
6. **Progress Tracking** - Remember playback position
7. **Multiple Language Support** - i18n for English/Korean toggle
8. **Dark Mode** - Theme switcher
9. **Build Process** - For production optimization
10. **Backend Integration** - Real content management system

---

## Questions to Ask Users

When receiving vague requests, clarify:

1. **"Add a feature"** - Should it be in admin panel or user-facing?
2. **"Change the design"** - Which page/component specifically?
3. **"Add videos"** - Do you have URLs or should I use sample videos?
4. **"Make it better"** - What specific aspect? Performance? UX? Features?
5. **"Fix the code"** - What's broken? What's the expected behavior?

---

## Related Files

- **index.html** - The entire application (read this first!)
- **README.md** - Brief project description
- **.git/** - Version control (do not modify directly)

---

## Quick Reference

### File Line Numbers for Common Edits

| What to Edit | Line Range |
|--------------|------------|
| Header logo/navigation | 34-67 |
| Hero banner | 91-112 |
| Content grid styling | 126-181 |
| Content data | 427-469 |
| Home page component | 475-513 |
| Detail page component | 515-554 |
| Video player | 556-593 |
| Admin panel | 595-625 |
| Page routing logic | 627-635 |

### CSS Gradient Colors Used

- Purple gradient: `#667eea → #764ba2`
- Blue gradient: `#1e3c72 → #2a5298`
- Pink gradient: `#f093fb → #f5576c`
- Orange gradient: `#fa709a → #fee140`

### Korean UI Text Reference

| English | Korean | Usage |
|---------|--------|-------|
| Admin | 관리자 | Header button |
| Watch Now | 지금 시청하기 | Hero CTA |
| Episodes | 에피소드 | Section header |
| Latest | 최신화 | Status badge |
| Complete | 완결 | Status badge |
| Watching | 시청중 | Status badge |
| Free | 무료 | Status badge |
| Edit | 수정 | Admin button |
| Delete | 삭제 | Admin button |
| Content Management | 콘텐츠 관리 | Admin modal title |

---

## Summary for AI Assistants

**Key Takeaways:**

1. Single HTML file architecture - everything in index.html
2. React via CDN - no build process
3. Korean language UI - maintain language consistency
4. Simple state management - useState hooks only
5. External video hosting - Google Cloud Storage samples
6. Client-side only - no backend or API calls
7. Edit CSS (lines 10-412) and React code (lines 417-638)
8. Test by refreshing browser - no compilation needed

**Before making changes:**
- Read index.html completely
- Understand the component you're modifying
- Check if changes affect mobile responsiveness
- Maintain Korean text for user-facing content
- Test video URLs are accessible

**After making changes:**
- Verify JSX syntax is correct
- Check browser console for errors
- Test on both desktop and mobile widths
- Commit with descriptive message
- Push to feature branch

---

*This CLAUDE.md file was generated on 2025-12-16 for the PlayMon video website project.*
