A CGI script for Gemini protocol servers, written in GNU AWK. It enables users to retrieve and display logbook entries from a text file based on a date specified in the URL. Think of it as a lightweight tool for serving date-specific logbook content over the Gemini protocol.

## Features
- Date-Based Retrieval: Extracts a date from the URL's PATH_INFO (e.g., /YYYY-MM-DD) to fetch the corresponding logbook entry.
- Input Validation: Ensures the date is in YYYY-MM-DD format and falls within the range of 2000–2099.
- Gemini Protocol Compliance: Outputs proper Gemini response headers and formats content in the Gemini text format.
- Entry Display: Shows the title and full content of the logbook entry matching the requested date.
- Fallback Behavior: If no entry matches the date, displays a list of all available logbook entries with clickable Gemini links.
- Link Generation: Automatically creates Gemini links for each logbook entry, enhancing navigation.

## Logbook File Format
The script expects a logbook file in the following format:

```
#### YYYY-MM-DD Title
Entry content line 1
Entry content line 2

#### YYYY-MM-DD Another Title
Another entry content
```
Each entry begins with a line starting with `#### `, followed by the date in `YYYY-MM-DD` format, a space, and the entry's title.

The entry's content follows on subsequent lines until the next `#### ` line or the end of the file.

It fetches the date, title and content of each entry, creating its own gemlog page.

## How It Works
### 1) Date Extraction and Validation:
- Grabs the first 10 characters of `PATH_INFO` after the leading `/` (e.g., `2023-10-15` from `/2023-10-15`). `PATH_INFO` is an environment variable given by the server. Check your server's documentation (YMMV).
- Validates the date format and range; if invalid, redirects to `/cgi-bin/journal` with a Gemini redirect response.

### 2) Logbook Processing:
- Reads the logbook file line by line.
- Collects all entry titles for potential listing.
- When an entry’s date matches the request, outputs a Gemini success header (20 text/gemini), a capsule header with a home link, the entry’s title as a level 3 heading (###), and its content.

### 3) No-Entry Fallback:
- If no match is found after processing the file, sends a success header and displays a message indicating no entry exists for the date.
- Lists all logbook entries with their dates and titles as Gemini links (e.g., `=> /YYYY-MM-DD Title`).

## Requirements
- GAWK, of course.
- Gemini server with CGI support. It must offer the URL info given after the script (check `PATH_INFO` variable).

## Setup Instructions
1. Place the Script: Copy the script to your Gemini server’s CGI directory (e.g., `/var/gemini/cgi-bin/`).
2. Set the Logbook Path: Edit the script to include the absolute path to your logbook file.
3. Make Executable: Run `chmod +x script` to grant execute permissions.
4. Access It: Visit `gemini://yourcapsule.com/cgi-bin/script/YYYY-MM-DD` in a Gemini client, replacing `YYYY-MM-DD` with the desired date.

## Security Note
As a CGI script, ensure it doesn’t expose security risks. This script only reads a predefined file and avoids executing user input, but always review CGI deployments for potential vulnerabilities.
