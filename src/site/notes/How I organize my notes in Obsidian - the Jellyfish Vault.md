---
{"dg-publish":true,"grade":3,"permalink":"/how-i-organize-my-notes-in-obsidian-the-jellyfish-vault/","dgPassFrontmatter":true}
---


Today, I'm sharing my minimal Obsidian starter vault meant to help you organize your notes primarily by metadata (Properties).

After trying to wrangle my notes to keep them all in the correct folder, I'm giving up and using file metadata and the Dataview plugin to organize my notes in Obsidian.

[View on GitHub](https://github.com/aaronjamesyoung/obsidian-jellyfish-vault)

## Concepts

1. All notes go into one folder, known as the "Jellyfish".
2. All notes have metadata in the YAML frontmatter that is used for organization. As of Obsidian 1.4, this is known as "Properties".
3. The vault contains "Dashboard" notes to aggregate and organize notes so you can easily find them and work with them.

## Using Properties for organizing your vault

I've loosely adopted some concepts from the PARA and Zettelkasten methods of notetaking, along with an original idea or two.

Notes in this vault contain the following properties:

* **context** - This refers to your "Areas of Responsibility". For me, everything I do falls into one of these categories: "Personal", "Family", "Work". You may have more categories than this - perhaps you have two jobs, or a hobby that's significant enough to warrant a spot on this list.
* **type** - What type of note is this? "Project", "Resource", and "Fleeting" are the options I use. "Fleeting" notes, a concept borrowed from the Zettelkasten method, are quick, often temporary notes to be refined and made permanent later. Sometimes I just want to write things down! "Resource" is a permanent note containing information, and "Project" is for the "home note" for a particular project.
* **status** - This particularly pertains to projects, but also Resource notes. I use five statuses: Active, Holding, Planned, Maybe, and Archive.
* **topic** - This is essentially a "tag". By adding a topic to each note, they will eventually collect and your notes will be grouped together on the Dashboard "Topics" note.

## What's included?

* The folder structure:
	* **Dashboards** - the dashboard notes are in here. They use the Dataview plugin to list your notes according to different parameters.
	* **Jellyfish** - this is the default home for all notes.
	* **Daily** - where the daily notes go, presuming you use them
	* **Utilities** - attachments and templates are configured to go in here.
* Template files: These use the Obsidian core Template plugin.
	* **Daily** - a template for your daily note
	* **Fleeting Note** - a minimal template for a fleeting note
	* **Note** - this contains the metadata fields for a standard permanent note in your vault.
	* **Project** - a customized version of the Note template, with some metadata preselected and a content structure added.
* Plugins: The following Community plugins are added:
	* **Dataview** - aggregate and visualize your notes
	* **Auto Template Trigger** - Prompts you to add a template when creating a new note

## Get Started

1. Clone the GitHub repository and open it in Obsidian as a vault.
2. Edit the "Note" template to update the options for "context", "type", and "status" to fit your workflow.
3. Edit the 'Project Note' template to update metadata and content as needed. Remove the explanatory text.
4. Create three new notes - a standard permanent note, a project, and a fleeting note. See how it prompts you to add a template right away? Choose the correct templates and update the file metadata before writing your notes.
5. Go through the Dashboard notes. You'll see your new notes listed, as well as the sample notes I added in the vault.
6. Remove sample / test notes and start actually using it!