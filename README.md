# Agenda Management System in VXML

This project is a spoken dialogue system for room reservation management, developed as part of a practical assignment on voice technologies (VXML and SRGS).

The main objective was to extend a basic VXML dialogue (`gestion_agenda_v0.vxml`) to create a complete application (`gestion_agenda_v1.vxml`) capable of managing a room reservation, which involved creating a complex SRGS grammar for time recognition (`grammaire_horaire.grxml`).

## Developed Files

While most grammars (numbers, dates, confirmation) and the base structure were provided, the core of this project consisted in developing the following two files:

1. **`gestion_agenda_v1.vxml`**: The main dialogue flow of the application.
2. **`grammaire_horaire.grxml`**: The speech recognition grammar for times.

---

## Operating Principle

### 1. `gestion_agenda_v1.vxml` (The Dialogue Flow)

This VXML file controls the conversation logic with the user. The v0 flow (which only asked for a subscriber number) was extended to include a complete reservation process:

* **Identification**: Asks for and validates a subscriber number.
* **Date Request**: Asks the user for the desired reservation date, using `grammaire_dates_v3.grxml`.
* **Date Confirmation**: Asks the user to confirm the recognized date.
* **Time Request**: Asks for the reservation time. This is the key step that uses our custom grammar.
* **Time Validation**: The VXML checks if the recognized time is valid (e.g., minutes < 60) and if it's within opening hours (e.g., 7:30 - 19:30). If invalid, it asks for the time again.
* **Final Confirmation**: Presents a complete summary (date and time) and asks for final confirmation before "saving" the reservation.

### 2. `grammaire_horaire.grxml` (Time Recognition)

This SRGS grammar was created from scratch to recognize a wide variety of time expressions in French and return a semantic result (`H`, `MN`, `NTOT_MIN`).

Its main logic is based on two rules:

1. **`#heure_base`**: Recognizes the main hour.
   * Handles special cases like "midi" (12) and "minuit" (0).
   * Handles numeric hours ("deux heures", "quinze heures") by calling `grammaire_nombre_v3.grxml`.
   * **Important**: Includes "hardcoded" rules for feminine forms that the number grammar doesn't handle, such as "**une** heure", "**vingt et une** heures", etc. (This was a key modification of the grammar you provided in our conversation).

2. **`#modificateur`**: Recognizes common modifiers that are added to the base hour:
   * "et quart" (+15 min).
   * "et demi" (+30 min).
   * "moins le quart" (-1H, +45 min).

The grammar also combines these rules to understand formats like "hour + number" (e.g., "dix heures **vingt**") or "hour + minus + number" (e.g., "trois heures **moins dix**").

---

## How to Test the Project

The project was designed to be tested with Optimtalk tools:

* **To test the grammar (`.grxml`):**
  Use `ot_grammar_tester.exe` to load a grammar file (e.g., `grammaire_horaire.grxml`) and test input phrases (like those in `gram_test.txt`) to see the semantic result.

* **To test the dialogue (`.vxml`):**
  Use `ot_vxml_interpreter.exe` to launch the complete dialogue application (e.g., `ot_vxml_interpreter.exe gestion_agenda_v1.vxml`).