import sqlite3
from hashlib import sha256

def create_table():
    conn = sqlite3.connect('users.db')
    cursor = conn.cursor()

    cursor.execute('''
        CREATE TABLE IF NOT EXISTS users (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            username TEXT NOT NULL,
            password TEXT NOT NULL,
            age INTEGER,
            gender TEXT
        )
    ''')
    conn.commit()
    conn.close()

def register_user(username, password, age=None, gender=None):
    conn = sqlite3.connect('users.db')
    cursor = conn.cursor()

    hashed_password = sha256(password.encode()).hexdigest()
    cursor.execute('''
        INSERT INTO users (username, password, age, gender)
        VALUES (?, ?, ?, ?)
    ''', (username, hashed_password, age, gender))
    conn.commit()
    print(f'Користувач {username} успішно зареєстрований.')

def display_users():
    conn = sqlite3.connect('users.db')
    cursor = conn.cursor()

    cursor.execute('SELECT * FROM users')
    users = cursor.fetchall()

    print("Список користувачів:")
    for user in users:
        print(user)

    conn.close()

def main():
    create_table()

    while True:
        print("\n1. Зареєструвати нового користувача")
        print("2. Показати список користувачів")
        print("3. Вийти")
        choice = input("Виберіть опцію (1/2/3): ")

        if choice == '1':
            username = input("Введіть ім'я користувача: ")
            password = input("Введіть пароль: ")
            age = input("Введіть вік (необов'язково): ")
            gender = input("Введіть стать (необов'язково): ")
            register_user(username, password, age, gender)

        elif choice == '2':
            display_users()

        elif choice == '3':
            break

        else:
            print("Невірний вибір. Спробуйте ще раз.")

if __name__ == "__main__":
    main()
