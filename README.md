# TECHNICAL DOCUMENTATION

{% hint style="info" %}
For a beginner-friendly guide to getting started with Private Cloud Director Community Edition, check out the Tutorials section: [Beginner’s Guide to Deploying PCD Community Edition](https://platform9.com/docs/private-cloud-director/private-cloud-director/beginner---s-guide-to-deploying-pcd-community-edition)
{% endhint %}

{% tabs %}
{% tab title="bash" %}
```bash
egrep "svm|vmx" /proc/cpuinfo
```
{% endtab %}
{% endtabs %}

```bash
egrep "svm|vmx" /proc/cpuinfo
```

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-cover data-type="image">Cover image</th></tr></thead><tbody><tr><td></td><td><a href=".gitbook/assets/WhatsApp Image 2024-12-16 at 12.50.28 PM (2).jpeg">WhatsApp Image 2024-12-16 at 12.50.28 PM (2).jpeg</a></td></tr><tr><td></td><td></td></tr></tbody></table>

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const message = "hello world";
console.log(message);
```
{% endtab %}
{% endtabs %}

```
// Some code

```

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const message = "hello world";
console.log(message)

```
{% endtab %}
{% endtabs %}

> \[!TIP] This shows up as a green box in GitBook.

{% code fullWidth="false" %}
```python
#!/usr/bin/env python3
"""Task: Simple Task Manager CLI
- single-file Python app
- demonstrates functions, classes, I/O, and CLI
"""
import json
import os
import sys
import datetime
import random
from typing import List, Dict, Optional
from dataclasses import dataclass, field

DB_FILENAME = "tasks_db.json"

@dataclass
class Task:
    id: int
    title: str
    description: str = ""
    created_at: str = field(default_factory=lambda: datetime.datetime.utcnow().isoformat())
    due: Optional[str] = None
    completed: bool = False
    tags: List[str] = field(default_factory=list)

    def to_dict(self) -> Dict:
        return {
            "id": self.id,
            "title": self.title,
            "description": self.description,
            "created_at": self.created_at,
            "due": self.due,
            "completed": self.completed,
            "tags": self.tags,
        }

    @staticmethod
    def from_dict(d: Dict) -> "Task":
        return Task(
            id=d["id"],
            title=d["title"],
            description=d.get("description", ""),
            created_at=d.get("created_at", datetime.datetime.utcnow().isoformat()),
            due=d.get("due"),
            completed=d.get("completed", False),
            tags=d.get("tags", []),
        )

def ensure_db_exists(filename: str = DB_FILENAME):
    if not os.path.exists(filename):
        with open(filename, "w") as f:
            json.dump({"tasks": []}, f)

def load_tasks(filename: str = DB_FILENAME) -> List[Task]:
    ensure_db_exists(filename)
    with open(filename, "r") as f:
        data = json.load(f)
    return [Task.from_dict(td) for td in data.get("tasks", [])]

def save_tasks(tasks: List[Task], filename: str = DB_FILENAME):
    with open(filename, "w") as f:
        json.dump({"tasks": [t.to_dict() for t in tasks]}, f, indent=2)

def next_id(tasks: List[Task]) -> int:
    if not tasks:
        return 1
    return max(t.id for t in tasks) + 1

def add_task(title: str, description: str = "", due: Optional[str] = None, tags: Optional[List[str]] = None):
    tasks = load_tasks()
    tid = next_id(tasks)
    task = Task(id=tid, title=title, description=description, due=due, tags=tags or [])
    tasks.append(task)
    save_tasks(tasks)
    print(f"Added task #{task.id}: {task.title}")

def list_tasks(show_all: bool = False, tag_filter: Optional[str] = None):
    tasks = load_tasks()
    if not show_all:
        tasks = [t for t in tasks if not t.completed]
    if tag_filter:
        tasks = [t for t in tasks if tag_filter in t.tags]
    if not tasks:
        print("No tasks found.")
        return
    for t in sorted(tasks, key=lambda x: (x.completed, x.due or "")):
        status = "✓" if t.completed else " "
        due = f" due:{t.due}" if t.due else ""
        tags = f" tags:{','.join(t.tags)}" if t.tags else ""
        print(f"[{status}] #{t.id} {t.title}{due}{tags}")

def complete_task(task_id: int):
    tasks = load_tasks()
    for t in tasks:
        if t.id == task_id:
            if t.completed:
                print(f"Task #{task_id} is already completed.")
                return
            t.completed = True
            save_tasks(tasks)
            print(f"Marked task #{task_id} as complete.")
            return
    print(f"Task #{task_id} not found.")

def delete_task(task_id: int):
    tasks = load_tasks()
    new_tasks = [t for t in tasks if t.id != task_id]
    if len(new_tasks) == len(tasks):
        print(f"Task #{task_id} not found.")
        return
    save_tasks(new_tasks)
    print(f"Deleted task #{task_id}.")

def edit_task(task_id: int, **kwargs):
    tasks = load_tasks()
    for t in tasks:
        if t.id == task_id:
            for k, v in kwargs.items():
                if hasattr(t, k) and v is not None:
                    setattr(t, k, v)
            save_tasks(tasks)
            print(f"Updated task #{task_id}.")
            return
    print(f"Task #{task_id} not found.")

QUOTES = [
   "Make it simple, but significant.",
   "Small steps every day.",
   "Focus on what you can control.",
   "Done is better than perfect.",
   "Progress, not perfection.",
]

def random_quote() -> str:
    return random.choice(QUOTES)

def pretty_print_task(task: Task):
    print("-" * 40)
    print(f"Task #{task.id}: {task.title}")
    print(f"Created: {task.created_at}")
    if task.due:
        print(f"Due: {task.due}")
    print(f"Completed: {task.completed}")
    if task.tags:
        print(f"Tags: {', '.join(task.tags)}")
    if task.description:
        print()
        print(task.description)
    print("-" * 40)

def show_task_details(task_id: int):
    tasks = load_tasks()
    for t in tasks:
        if t.id == task_id:
            pretty_print_task(t)
            return
    print(f"Task #{task_id} not found.")

def import_sample_data():
    sample = [
        {"title": "Write report", "description": "Monthly financials", "due": "2025-09-05", "tags": ["work"]},
        {"title": "Grocery shopping", "description": "Milk, Eggs, Bread", "due": None, "tags": ["personal"]},
        {"title": "Plan vacation", "description": "Look up flights", "due": "2025-12-01", "tags": ["personal", "travel"]},
    ]
    tasks = load_tasks()
    base = next_id(tasks)
    for i, s in enumerate(sample):
        tasks.append(Task(id=base + i, title=s["title"], description=s["description"], due=s["due"], tags=s["tags"]))
    save_tasks(tasks)
    print("Imported sample data.")

def search_tasks(keyword: str) -> List[Task]:
    tasks = load_tasks()
    keyword = keyword.lower()
    return [t for t in tasks if keyword in t.title.lower() or keyword in t.description.lower()]

def run_demo_mode():
    print("Running demo mode — showing a random quote and upcoming tasks.")
    print(random_quote())
    list_tasks(show_all=False)

def print_help():
    print("Usage:")
    print("  python tasker.py add \"Title\" [--desc \"desc\"] [--due YYYY-MM-DD] [--tags a,b]")
    print("  python tasker.py list [--all] [--tag TAG]")
    print("  python tasker.py complete ID")
    print("  python tasker.py delete ID")
    print("  python tasker.py show ID")
    print("  python tasker.py edit ID --title \"New\" --desc \"...\"")
    print("  python tasker.py import-sample")
    print("  python tasker.py demo")

def parse_args(argv: List[str]):
    if len(argv) < 2:
        print_help()
        return
    cmd = argv[1]
    if cmd == "add":
        title = argv[2] if len(argv) > 2 else input("Title: ")
        desc = ""
        due = None
        tags = []
        # crude parsing
        for i, a in enumerate(argv[2:], start=2):
            if a == "--desc" and i + 1 < len(argv):
                desc = argv[i + 1]
            if a == "--due" and i + 1 < len(argv):
                due = argv[i + 1]
            if a == "--tags" and i + 1 < len(argv):
                tags = argv[i + 1].split(",")
        add_task(title, desc, due, tags)
    elif cmd == "list":
        show_all = "--all" in argv
        tag = None
        if "--tag" in argv:
            idx = argv.index("--tag")
            if idx + 1 < len(argv):
                tag = argv[idx + 1]
        list_tasks(show_all=show_all, tag_filter=tag)
    elif cmd == "complete":
        if len(argv) < 3:
            print("Missing ID")
            return
        complete_task(int(argv[2]))
    elif cmd == "delete":
        if len(argv) < 3:
            print("Missing ID")
            return
        delete_task(int(argv[2]))
    elif cmd == "show":
        if len(argv) < 3:
            print("Missing ID")
            return
        show_task_details(int(argv[2]))
    elif cmd == "edit":
        if len(argv) < 3:
            print("Missing ID")
            return
        tid = int(argv[2])
        kwargs = {}
        for i, a in enumerate(argv[3:], start=3):
            if a == "--title" and i + 1 < len(argv):
                kwargs["title"] = argv[i + 1]
            if a == "--desc" and i + 1 < len(argv):
                kwargs["description"] = argv[i + 1]
            if a == "--due" and i + 1 < len(argv):
                kwargs["due"] = argv[i + 1]
            if a == "--tags" and i + 1 < len(argv):
                kwargs["tags"] = argv[i + 1].split(",")
        edit_task(tid, **kwargs)
    elif cmd == "import-sample":
        import_sample_data()
    elif cmd == "demo":
        run_demo_mode()
    else:
        print_help()

if __name__ == "__main__":
    parse_args(sys.argv)

```
{% endcode %}

{% tabs %}
{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```yaml
paths:
  /page/{id}:
    get:
      tags:
        - Page
      summary: Read page
      description: "Read a page including its content in [Darkdown](https://docs.developerhub.io/support-center/exporting-documentation#darkdown) or text format. Rate limit: 600 in 1 minute."
      operationId: read_page
      parameters:
        - name: id
          in: path
          description: Page ID
          required: true
          schema:
            type: integer
        - name: format
          in: query
          description: Format type (Default `darkdown`)
          required: false
          schema:
            type: string
            enum:
              - darkdown
              - text
      responses:
        200:
          description: OK
          headers:
            X-RateLimit-Limit:
              $ref: '#/components/headers/X-RateLimit-Limit'
            X-RateLimit-Remaining:
              $ref: '#/components/headers/X-RateLimit-Remaining'
            X-RateLimit-Reset:
              $ref: '#/components/headers/X-RateLimit-Reset'
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Page'
        403:
          description: Access denied
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/AccessDenied'
        429:
          description: Too many requests
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TooManyRequests'
      deprecated: false
  /documentation/{id}/page:
    post:
      tags:
        - Page
      summary: Create a page
      description: "Creates a page with draft contents. To insert in a pre-existing category, set the `categoryTitle`. Rate limit: 10800 in 1 hour."
      operationId: create_page
      parameters:
        - name: id
          in: path
          description: Documentation ID
          required: true
          schema:
            type: integer
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              description: Page object
              properties:
                title:
                  type: string
                  description: Title of the page
                  example: "Getting Started"
                slug:
                  type: string
                  description: Slug in the URL. Generated if it was not provided
                  example: "getting-started"
                content:
                  type: string
                  description: Draft contents of the page in [Darkdown format](https://docs.developerhub.io/support-center/exporting-documentation#darkdown)
                  example: "Let's start here\n\n##Step 1\nWelcome to **DeveloperHub**\n"
                categoryTitle:
                  type: string
                  description: To append the page to a category, provide the pre-existing category title here (case insensitive)
                  example: Installation
                message:
                  type: string
                  description: Page history message
                  default: Created using API
              required:
                - title
                - content
      responses:
        200:
          description: OK
          headers:
            X-RateLimit-Limit:
              $ref: '#/components/headers/X-RateLimit-Limit'
            X-RateLimit-Remaining:
              $ref: '#/components/headers/X-RateLimit-Remaining'
            X-RateLimit-Reset:
              $ref: '#/components/headers/X-RateLimit-Reset'
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Page'
        403:
          description: Access denied
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/AccessDenied'
        415:
          description: Unsupported content-type
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UnsupportedContentType'
        429:
          description: Too many requests
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TooManyRequests'
      deprecated: false
```
{% endcode %}
{% endtab %}
{% endtabs %}

<figure><img src=".gitbook/assets/WhatsApp Image 2024-12-16 at 12.50.28 PM (2).jpeg" alt=""><figcaption><p>Hello there THis is a caption</p></figcaption></figure>



```
// Some code
print("Ganpati bappa moriya")
```

[https://app.gitbook.com/o/GWuWQ8OGNaWCrXd7om5R/s/3nJeLiLMYpTpHk6LrO80/\~/changes/11/#second-tab](./#second-tab)

{% tabs %}
{% tab title="First Tab" %}

{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="First Tab" %}

{% endtab %}

{% tab title="Secondddd Tab" %}

{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Windows" %}
THIS IS THE FIRST TAB
{% endtab %}

{% tab title="Linux" %}
tHIS IS THE SECOND TAB
{% endtab %}

{% tab title="THird Tab" %}
tHIS IS THE SECOND TAB
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="First Tab" %}

{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="First Tab" %}

{% endtab %}

{% tab title="Second Tab" %}
fdsafdsa
{% endtab %}
{% endtabs %}

|   |   |   |
| - | - | - |
|   |   |   |
|   |   |   |
|   |   |   |
