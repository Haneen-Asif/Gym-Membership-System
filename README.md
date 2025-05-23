# Gym-Membership-System 
-- Create the Users table
CREATE TABLE Users (
    user_id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    email TEXT NOT NULL UNIQUE,
    password TEXT NOT NULL
);

-- Create the Membership_Plans table
CREATE TABLE Membership_Plans (
    plan_id INTEGER PRIMARY KEY AUTOINCREMENT,
    plan_name TEXT NOT NULL,
    plan_description TEXT,
    plan_price REAL NOT NULL
);

-- Create the Trainers table
CREATE TABLE Trainers (
    trainer_id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    specialization TEXT,
    contact_info TEXT
);

-- Create the Members table
CREATE TABLE Members (
    member_id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER NOT NULL UNIQUE,
    membership_plan_id INTEGER,
    start_date TEXT NOT NULL,
    end_date TEXT NOT NULL,
    payment_status TEXT NOT NULL CHECK (payment_status IN ('Paid', 'Pending')),
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (membership_plan_id) REFERENCES Membership_Plans(plan_id) ON DELETE SET NULL
);

-- Create the Sessions table
CREATE TABLE Sessions (
    session_id INTEGER PRIMARY KEY AUTOINCREMENT,
    trainer_id INTEGER NOT NULL,
    session_time TEXT NOT NULL,
    member_id INTEGER NOT NULL,
    session_status TEXT NOT NULL CHECK (session_status IN ('Scheduled', 'Completed')),
    FOREIGN KEY (trainer_id) REFERENCES Trainers(trainer_id) ON DELETE CASCADE,
    FOREIGN KEY (member_id) REFERENCES Members(member_id) ON DELETE CASCADE
);

-- Create the Payments table
CREATE TABLE Payments (
    payment_id INTEGER PRIMARY KEY AUTOINCREMENT,
    member_id INTEGER NOT NULL,
    amount REAL NOT NULL,
    payment_date TEXT NOT NULL DEFAULT (datetime('now')),
    payment_method TEXT NOT NULL,
    FOREIGN KEY (member_id) REFERENCES Members(member_id) ON DELETE CASCADE
);

-- Create indexes for faster queries
CREATE INDEX idx_payments_member_id ON Payments(member_id);
CREATE INDEX idx_sessions_member_id ON Sessions(member_id);
CREATE INDEX idx_sessions_trainer_id ON Sessions(trainer_id);
CREATE INDEX idx_members_user_id ON Members(user_id);
