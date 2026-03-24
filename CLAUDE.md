# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

ProcessWire module that extends FormBuilder with a processor action to forward form submissions to the Revinate contact API (`https://contact-api.inguest.com/api/add-contacts-to-lists`). It hooks into `FormBuilderProcessor::processInputDone` to capture form data after submission, maps form fields to Revinate's expected contact fields, and POSTs the data as JSON.

## Architecture

Single-file module (`FormBuilderProcessorRevinate.module`) that extends `FormBuilderProcessorAction`. Key parts:

- **`processReady()`** — Hooks into FormBuilder's `processInputDone` event. Collects form field values, maps them to Revinate fields, and sends them via `WireHttp::post()`.
- **`getConfigInputfields()`** — Provides admin UI: list tokens (one per line), field renaming map (`revinate_name=form_field`), and a debug toggle.
- **`$revinateFields`** — Whitelist of accepted Revinate field names (email, first_name, last_name, address fields, phone).
- **`textToArray()`** — Parses newline-delimited config text into arrays (key=value pairs or numeric lists).

## Dependencies

- **ProcessWire** (host CMS)
- **FormBuilder >= 0.4.5** (commercial ProcessWire module)
- **PHP >= 7.3**
- Installed via Composer using `wireframe-framework/processwire-composer-installer`

## Development Notes

- No build step, tests, or linting — this is a single PHP file module.
- Debug logging writes to ProcessWire's `fb-revinate` log file when the `revinate_debug` checkbox is enabled in the module config.
- The `email` field is required; if not found in the form submission, the API call is skipped entirely.
