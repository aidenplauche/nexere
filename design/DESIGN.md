# Design
[Back to README.md](/README.md)

### Problem Statement
- There are not many options for taking notes in a well-organized, searchable, shared, and (subjectively) aesthetically pleasing format.
- Existing platforms are missing at least some of the desired features:
  - Storing multiple documents in a hierarchical format
  - Easily linking to other documents
  - Global searchability of documents
  - Portability of formatting
  - Collaborative, real-time editing
  - Local app, local server, free (i.e., not a SaaS product)

### Proposed Solution
- Primary goal: "Folders that you can write in" (or, documents containing other documents) that are shared with multiple people.
- Solution: Collection of linked [markdown](https://www.markdownguide.org/) files (wiki format) backed by a WebSocket-based server.

#### Why Markdown?
- Widely supported specification with relatively simple rules for rendering and functionality
- Supported by other tools and platforms (Notepad, Obsidian, etc.)
- Feature-rich (image and file linking, text formatting, etc.)
- Extensible (HTML, Mermaid diagrams, etc.)

#### Why WebSocket?
- Backbone for live collaboration and conflict resolution
- Standard for solving the aforementioned problem
- Not explicitly required for sharing files, but requirement for real-time
- Going real-time has other benefits (e.g., little need to restrict editing or resolve conflict)

---

### Primary Design Requirements
A. File Management
  1. Create, rename, delete, duplicate, and move markdown files and folders
  2. Upload and download non-markdown files (images, videos, etc.)
  3. Preview supported non-markdown files
  4. Real-time replication of any changes to file hierarchy (incl. new files)
B. Markdown Editor
  1. Styled text editor with full support for markdown specification
  2. Extensions to markdown specification (e.g., HTML renderer / image embed)
  3. Hyperlinking to other files within markdown files
  4. Real-time sending and receiving of user text edits
C. Configuration
  1. Connect to and configure remote server(s)
  2. Create a local "user" (nickname / identity) on a connected server
  3. Application settings (as desired - e.g., app theme)
  4. View Application info (disk usage, version, etc.)
D. Administration (server-side only)
  1. Create, rename, and delete "projects" on the server (units of organization)
  2. Lock / unlock files from being edited or removed
  3. Create, view, remove, and configure project backups
  4. Set a simple password required before a user is "trusted"

### Design Notes:
- Files are actually stored flat, with their name as their ID
- SQLite Database acts as translation layer between file ID and hierarchical structure
- Any file can have children files (no restriction on what can act as a folder)
- Users cannot have the same username or exact hex color code (we decided)

---

### Software Architecture
- Desired application architecture - one binary is server and client
- Three connection modes:
  - Local - no server / websockets / etc needed, just runs locally as a file browser + markdown editor
    - i.e., CLIENT enabled, SERVER disabled
  - P2P / LAN mode - user acts as a server, other users connect via IP / port
    - i.e., CLIENT enabled, SERVER enabled
  - Server mode - spawns server infra headlessly, other users and server owner connect to server IP / port
    - i.e., CLIENT disabled, SERVER enabled

---

### Future Enhancements
- Extensions (dice roller, stop watch, template)
- Clickable images (like an interactive map)
- File permissions (read-only files and hidden files)
