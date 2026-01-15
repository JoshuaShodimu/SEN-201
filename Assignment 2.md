Project Selection: Library Management System (LMS)
The goal of this project is to automate the tracking of books and their availability within a local library.
2. The Full SDLC Process
Phase 1: Requirement Analysis
• Problem: The library currently uses manual logs, leading to lost books and data entry errors.
• Functional Requirements: The system must allow users to register books, track which books are borrowed, and update book availability.
• Stakeholders: Librarian (Admin) and Library Members.
Phase 2: Design & Nomenclature
To ensure the implementation matches the design, we establish these strict naming conventions:
• Database Table: BookRecord
• Primary Keys: book_isbn (String), member_id (Integer)
• Status Indicators: is_available (Boolean), due_date (Date)
• System Functions: register_book(), issue_book(), return_book()
Phase 3: Implementation Strategy
The system is built using a structured approach where the Nomenclature defined in Phase 2 is applied directly to the class structures and database schemas. The logic follows a "Service-Oriented" pattern to separate user actions from data storage.
Phase 4: Testing (Verification & Validation)
• Unit Testing: Testing the register_book() function to ensure it correctly populates the BookRecord table.
• Logic Testing: Verifying that a book’s is_available status cannot be changed to "True" if it is currently flagged as borrowed.
Phase 5: Deployment & Maintenance
The project is pushed to GitHub for version control. Maintenance involves monitoring the database for performance and adding new features like "Late Fee Calculation" in future sprints.
