# SAIL Presentation

Hi everyone! We're Solar Flow.

I'm Caleb, a **rising senior** at **UW Madison** studying **Comp Sci** and **Math**. I'm from the **Twin Cities, Minnesota**.

...

**How's your downloads folder?** There's a good chance it looks something like this, which reveals a **fundamental flaw** with how we think about files.

Let's break down what we're trying to solve.
First, we're **spending way too much time** on file management, both **setting up systems** and  **maintaining them** as they **break down**.
Second, **file search is frustrating**. You know that file exists, you can even picture it, but the system can only find it if you **remember the exact filename**.
Third, we're **asking** **humans to think like computers**. File systems expect everyone to think in **rigid hierarchies**, and that's just **not** how most people's **minds work**.

So here's our solution.
We use **OpenAI's tools** to **completely automate** every aspect of **file management**.
Each user gets their own **personalized filing structure**.
And when you need to find something? Just **search by what's in the file**. No need to remember exact filenames or where you put something. Sort of like a Google for your file system.

Under the hood, we have both a backend and a frontend running **on the user's device**.
The backend uses Python and the Quart web framework, connecting to a SQLite database. And the frontned uses Swift and SwiftUI for a native MacOS app.

We've also used lots of OpenAI tools.
We used GPT-5-nano for **extracting summaries and keywords from file content**, Whisper for **creating transcripts** of audio and video files, and text-embedding-3 for **creating our vector database**.
We also used ChatGPT, Deep Research, and Codex CLI for helping us **ideate, research technologies, and craft some of our code**.