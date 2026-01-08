# MediaAnnotator

**MediaAnnotator** is a self-hosted, web-based tool for annotating video, audio, and PDF files. It allows you to create timestamped notes for media and region-based annotations for documents, all organized within a unified interface. It supports tag management, hierarchical metadata inheritance, and collection grouping.

## Features
* **Multi-Format Support**: Annotate Videos (`.mp4`, `.webm`, `.mov`), Audio (`.mp3`, `.wav`), and Documents (`.pdf`).
* **Smart Metadata**: Define metadata (Author, Collection, Tags) at the **folder level** and have it automatically inherited by all files inside.
* **PDF Region Capture**: Select and snapshot specific regions of a PDF page to attach to your annotations.
* **Collections**: Organize multi-part series with automatic "Part X of Y" logic and a dedicated collection browser.
* **Tagging System**: Centralized tag library with auto-complete and filtering (e.g., `#research`, `#todo`).
* **Import/Export**: Import SRT subtitles as annotations; export annotations as text, SRT, or a ZIP archive containing screenshots.
* **Portable**: Can be run as a standard Python app or built into a single-file executable.


---

## Installation
### Option A: Running from Source
Prerequisites: Python 3.8+

1. **Clone and Setup Environment**
    ```bash
    git clone https://github.com/your-repo/MediaAnnotator.git
    cd MediaAnnotator
    python3 -m venv .venv
    source .venv/bin/activate  # or .venv\Scripts\activate on Windows
    pip install -r requirements.txt

    ```
2. **Initialize Database**
    ```bash
    export FLASK_APP=run.py
    flask db init
    flask db migrate -m "Initial schema"
    flask db upgrade
    ```
3. **Create a User**
    ```bash
    python run.py --manage-users --action add --username admin --name "Admin User"
    # You will be prompted to set a password
    ```

4. **Run the Server**
    ```bash
    ./run  # or python run.py
    ```



### Option B: Portable/Frozen Version
If you have downloaded the single-file executable (e.g., `MediaAnnotator` or `MediaAnnotator.exe`):

1. Place the executable in your desired folder.
2. Run it via the command line to create your first user (see *Management CLI* below).
3. Start the application by double-clicking it or running `./MediaAnnotator`.
4. Open your browser to `http://localhost:8000`.

---

## Configuration
The application is configured via a `.env` file in the project root (or next to the executable) --- please look at it (and the comments contained therein) for more information on the required an optional configuration parameters.


---
## Management & Upgrades (CLI)
The `run.py` script (and the frozen executable) includes a Command Line Interface (CLI) for maintenance tasks.

### 1. User Management
You can add, remove, or list users without direct database access.

* **Add a User:**
    ```bash
    # Python
    python run.py --manage-users --action add --username myuser --name "My Name"

    # Executable
    ./MediaAnnotator --manage-users --action add --username myuser
    ```
* **Remove a User:**
    ```bash
    python run.py --manage-users --action remove --username myuser
    ```
* **List Users:**
    ```bash
    python run.py --manage-users --action list
    ```



### 2. Upgrading & Database Migration (`--migrate-db`)
When upgrading the **frozen/portable version**, you might replace the `.exe` file but keep your existing `annotations.db`. Because the internal migration ID of the new `.exe` often differs from your old database, you may see a "Can't locate revision" error.

To fix this, run the new version once with the `--migrate-db` flag. This resets the version stamp, allowing the new app to re-verify the schema and attach correctly.

```bash
./MediaAnnotator --migrate-db
```

*After running this successfully, start the application normally.*


---

## User Guide

### 1. The Interface
* **Sidebar**: Contains your file library, recently watched files, tag browser, and collection browser. Toggle it with `[` or the arrow button.
* **Viewer**: The central area for the Video Player or PDF Reader.
* **Metadata Panel**: Located on the right (or bottom on small screens). Used to edit properties for the currently selected file.

### 2. Metadata & Inheritance
MediaAnnotator uses a hierarchical metadata system to save you time.

* **Folder Metadata**: You can right-click on a **Folder** in the sidebar to edit its details (Author, Collection, Tags, etc.).
* **Inheritance**: Files inside that folder will automatically "inherit" these values as defaults.
* *Example*: If you set the Author of `Movies/SciFi` to "Ridley Scott", all files inside will show "Ridley Scott" as the default Author.
* **Overriding**: Click into a greyed-out (inherited) field on a specific file to type a new value. This overrides the folder default for that specific file.


* **Collections & Parts**:
* Assign a **Collection Name** to group related files.
* Use the **Part** fields (e.g., Part `1` of `3`) to order them.
* If you set the "Total Parts" on a folder, files inside will inherit that total count.



### 3. Annotating Media
* **Video/Audio**:
    * Navigate to the timestamp you want to annotate.
    * Press `a` to create an annotation.
    * Press `A` (Shift+a) to capture the current video frame as a thumbnail attachment.


* **PDF**:
    * Navigate to the desired page.
    * **Text Annotation**: Press `a` to annotate the whole page.
    * **Region Annotation**: Hold `Shift` and **Click & Drag** a box over a specific part of the PDF page. This captures that region as an image attachment for your note.





### 4. Tags & Search
* **Tags**: Add tags to files using the `#` symbol (e.g., `#important`). Tags are normalized (lowercase) automatically.
* **Filtering**:
* Use the **Sidebar Search** to filter by filename, summary text, or tags.
* Click the **Tag Tab** in the sidebar to see all tags and how many files use them. Ctrl+Click multiple tags to filter for files that contain *any* of the selected tags.



---

## Keyboard Shortcuts
Press `?` inside the application to see the cheat sheet.

<table>
  <thead>
    <tr> <th>Key</th> <th>Action</th> </tr>
  </thead>
  <tbody>
    <tr> <td colspan="2"><strong>General</strong></td> </tr>
    <tr> <td><code>[</code></td> <td>Toggle Sidebar</td> </tr>
    <tr> <td><code>?</code></td> <td>Toggle Shortcuts Overlay</td> </tr>
    <tr> <td><code>Esc</code></td> <td>Close Modals / Search / Cancel Edit</td> </tr>
    <tr>
      <td colspan="2"><strong>Playback (Video/Audio)</strong></td>
    </tr>
    <tr>
      <td><code>Space</code></td>
      <td>Play / Pause</td>
    </tr>
    <tr>
      <td><code>ArrowRight</code> / <code>ArrowLeft</code></td>
      <td>Seek +5s / -5s</td>
    </tr>
    <tr>
      <td><code>0</code> - <code>9</code></td>
      <td>Seek to 0% - 90% of duration</td>
    </tr>
    <tr>
      <td><code>ArrowUp</code> / <code>ArrowDown</code></td>
      <td>Volume Up / Down</td>
    </tr>
    <tr>
      <td><code>&gt;</code> / <code>&lt;</code></td>
      <td>Speed Up / Down</td>
    </tr>
    <tr>
      <td><code>c</code></td>
      <td>Toggle Captions (if SRT available)</td>
    </tr>
    <tr>
      <td colspan="2"><strong>PDF Navigation</strong></td>
    </tr>
    <tr>
      <td><code>ArrowRight</code></td>
      <td>Next Page</td>
    </tr>
    <tr>
      <td><code>ArrowLeft</code></td>
      <td>Previous Page</td>
    </tr>
    <tr>
      <td><code>Shift</code> + <code>Drag</code></td>
      <td>Capture Region (Mouse)</td>
    </tr>
    <tr>
      <td colspan="2"><strong>Annotation</strong></td>
    </tr>
    <tr>
      <td><code>a</code></td>
      <td>Start Annotation (at current time/page)</td>
    </tr>
    <tr>
      <td><code>Shift</code> + <code>a</code></td>
      <td>Capture Frame (Video) and Annotate</td>
    </tr>
    <tr>
      <td><code>PageUp</code> / <code>PageDown</code></td>
      <td>Jump to Previous / Next Annotation</td>
    </tr>
    <tr>
      <td><code>Enter</code></td>
      <td>Save Annotation (while editing)</td>
    </tr>
  </tbody>
</table>
