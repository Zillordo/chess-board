# Chessboard for chess.com

## How to Start

1. `npm install`
2. `npm run dev`

## How to use

1. App renders chess board with a left side panel
2. Each square when clicked, will be recorded as a position of {1-8} number plus {a-h} letters
3. Adding number of moves that will not fit to the element will make the moves section scrollable and it keeps the scroll pinned to the bottom
4. It will be displayed on the right side panel as chess moves would
5. With a right click (or ctrl + click, on mobile its long press) the square gets highlighted with red. Click anywhere on the board will reset the highlight
6. Chessboard has different themes which can be selected from the bottom select of the right side panel
7. The whole page has dark and light themes which can be toggled with the (sun/moon) button at the right side panel

## Technologies used

1. Vite: I have use Vite as it provides fast and easy setup for the project.
2. Vue: As per task request I have used Vue framework for this application
3. Tailwind: I have decided to use tailwind as it provides easy and reusable design system with a lot of variability. It has allowed me to setup simple css variables that I can use with tailwind to easily change the dark/light theme or chessboard theme.
4. Radix-vue: As I have some experience with using radix in react and it provides great headless components. (Select)
