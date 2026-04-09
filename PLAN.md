# Prompt: Build "NEO-INTERVIEW" Prep Tracker (Expo/Brutalism)

Build a full-stack mobile application using **Expo (React Native)** called **"NEO-INTERVIEW"**. This app is a 4-week intensive technical interview preparation tool.

## 1. Design System: BRUTALISM (MANDATORY)
The design must strictly follow **Brutalist** principles:
- **Borders**: Thick, solid black borders on everything (`borderWidth: 3`, `borderColor: '#000'`).
- **Shadows**: No soft blurs. Use hard, offset solid shadows.
    - *Implementation Tip*: Use a `View` with `backgroundColor: '#000'` offset behind your main containers to create the "Hard Shadow" effect.
- **Colors**: High-contrast palette.
    - Primary: Neon Yellow (`#F0F33E`)
    - Secondary: Electric Blue (`#3E64F3`)
    - Accent: Hot Pink (`#F33E91`)
    - Background: Stark White (`#FFFFFF`) or Light Grey (`#E0E0E0`)
- **Typography**: Use Monospace fonts for all data and UI elements (e.g., `SpaceMono`).
- **Layout**: Sharp corners (0 border radius), oversized buttons, and visible "raw" structure.

## 2. Core Features & Logic

### A. Curriculum & Study Mode
- **Structure**: 4 Weeks of content. Each week contains a list of "Questions" (topics).
- **Difficulty Badges**: Every question has a difficulty rating (**Easy**, **Medium**, or **Hard**).
- **Filtering**: A top-level search bar and difficulty filter buttons (All, Easy, Medium, Hard).
- **Resources**: Each question includes a list of study resources (Videos, Articles, Docs, etc.).
- **Resource UI**: Style resources as **Brutalist Buttons** that open URLs via `Linking.openURL`. Use `lucide-react-native` icons based on type.

### B. Interview Mode (The "Interviewer")
- **Toggle**: A switch to enter "Interview Mode".
- **Hidden Resources**: Resources are hidden during the interview to simulate a real environment.
- **Timer System**:
    - **Stopwatch Mode**: Counts up from 00:00.
    - **Countdown Mode**: Presets for 15m, 25m, and 45m.
    - **Visual Warning**: When < 60s remains in countdown, the timer background flashes Hot Pink and the text pulses.
- **Writing Space**: A dynamic `TextInput` that grows as the user types their explanation.
- **AI Review (Gemini API)**:
    - Integrate `@google/genai`.
    - Prompt: "Act as a Senior Technical Interviewer. Review this response for the question [Q]. Provide a rating (1-10) and constructive feedback."
    - Display the AI feedback in a high-contrast "Interviewer Note" box.

### C. Prep Kit & Activity
- **Prep Kit**: A curated list of resources and "Pro-Tips" for the interview.
- **Activity Calendar**: A 30-day grid (Heatmap) showing study consistency. Highlight days where tasks were completed.

### D. Tech Stack & Persistence
- **Auth**: Firebase Google Login.
- **Database**: Firestore to sync `completedTasks`, `userResponses`, `aiFeedback`, and `reminders`.
- **Icons**: `lucide-react-native`.

## 3. Data Structure

```typescript
export interface Resource {
  title: string;
  url: string;
  type: 'video' | 'article' | 'docs' | 'interactive' | 'course' | 'tool';
}

export interface Question {
  id: string;
  text: string;
  difficulty: 'Easy' | 'Medium' | 'Hard';
  resources: Resource[];
}

export const curriculum = [
  {
    week: 1,
    title: "JS/TS Core, OOP & Basic DSA",
    questions: [
      { 
        id: "q1", 
        text: "What are the four pillars of OOP? Explain them.",
        difficulty: "Medium",
        resources: [
          { title: "The 4 Pillars of OOP (Video)", url: "https://www.youtube.com/watch?v=pTB0EiLXUC8", type: 'video' },
          { title: "OOP Principles (Article)", url: "https://www.freecodecamp.org/news/four-pillars-of-object-oriented-programming/", type: 'article' },
          { title: "Design Patterns & OOP", url: "https://refactoring.guru/design-patterns", type: 'docs' }
        ]
      },
      { 
        id: "q2", 
        text: "What is a class in JavaScript? How does it differ from a prototype?",
        difficulty: "Medium",
        resources: [
          { title: "Classes vs Prototypes (MDN)", url: "https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain", type: 'docs' },
          { title: "JS Classes Explained (Video)", url: "https://www.youtube.com/watch?v=2ZphE5HcQPQ", type: 'video' },
          { title: "JS.info: Prototypes", url: "https://javascript.info/prototypes", type: 'docs' }
        ]
      },
      { 
        id: "q3", 
        text: "What is the 'this' keyword in JavaScript, and how does its context change?",
        difficulty: "Medium",
        resources: [
          { title: "Understanding 'this' (MDN)", url: "https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this", type: 'docs' },
          { title: "The 'this' Keyword (Video)", url: "https://www.youtube.com/watch?v=gvicrj31JOM", type: 'video' }
        ]
      },
      { 
        id: "q4", 
        text: "What is the difference between Composition and Inheritance?",
        difficulty: "Medium",
        resources: [
          { title: "Composition vs Inheritance (Video)", url: "https://www.youtube.com/watch?v=nnwD5Lwwqdo", type: 'video' },
          { title: "Design Patterns (Article)", url: "https://www.freecodecamp.org/news/composition-vs-inheritance-in-javascript/", type: 'article' }
        ]
      },
      { 
        id: "q5", 
        text: "TypeScript: What is the difference between an interface and a type?",
        difficulty: "Easy",
        resources: [
          { title: "Interface vs Type (Official Docs)", url: "https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces", type: 'docs' },
          { title: "TS: Type vs Interface (Video)", url: "https://www.youtube.com/watch?v=Idf0zh9f3qQ", type: 'video' }
        ]
      },
      { 
        id: "q6", 
        text: "TypeScript: What are Generics? Can you write a simple generic function?",
        difficulty: "Medium",
        resources: [
          { title: "TS Generics (Official Docs)", url: "https://www.typescriptlang.org/docs/handbook/2/generics.html", type: 'docs' },
          { title: "Generics in 100 Seconds (Video)", url: "https://www.youtube.com/watch?v=nViEqpgwxHE", type: 'video' }
        ]
      },
      { 
        id: "q7", 
        text: "What is a Hash Map, and how do you implement one?",
        difficulty: "Hard",
        resources: [
          { title: "Hash Tables (Video)", url: "https://www.youtube.com/watch?v=knV86IfGISw", type: 'video' },
          { title: "JS Map Object (MDN)", url: "https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map", type: 'docs' }
        ]
      },
      { 
        id: "q8", 
        text: "Explain the time complexity (Big O) of finding an item in an Array vs. an Object.",
        difficulty: "Easy",
        resources: [
          { title: "Big O Notation (Video)", url: "https://www.youtube.com/watch?v=itnHiK6wA5s", type: 'video' },
          { title: "Big O Cheat Sheet", url: "https://www.bigocheatsheet.com/", type: 'interactive' }
        ]
      },
      { 
        id: "q9", 
        text: "Practical: Write a function to find the two numbers that add up to a target (Two Sum).",
        difficulty: "Easy",
        resources: [
          { title: "Two Sum Problem (LeetCode)", url: "https://leetcode.com/problems/two-sum/", type: 'interactive' },
          { title: "Two Sum Solution (Video)", url: "https://www.youtube.com/watch?v=KLlXCFG5Tk0", type: 'video' }
        ]
      },
      { 
        id: "q10", 
        text: "Practical: Write a function to check if a string is a palindrome.",
        difficulty: "Easy",
        resources: [
          { title: "Palindrome Check (Video)", url: "https://www.youtube.com/watch?v=0sWShKIJoo4", type: 'video' }
        ]
      },
      { 
        id: "q11", 
        text: "Practical: Reverse a string without using the built-in .reverse() method.",
        difficulty: "Easy",
        resources: [
          { title: "Reverse String (Video)", url: "https://www.youtube.com/watch?v=u6_v9S6S62U", type: 'video' }
        ]
      },
    ],
  },
  {
    week: 2,
    title: "React, Next.js, React Native",
    questions: [
      { 
        id: "q12", 
        text: "Explain the Virtual DOM and React’s reconciliation process.",
        difficulty: "Medium",
        resources: [
          { title: "React Reconciliation (Docs)", url: "https://react.dev/learn/preserving-and-resetting-state", type: 'docs' },
          { title: "Virtual DOM Explained (Video)", url: "https://www.youtube.com/watch?v=BYbgopx44vo", type: 'video' }
        ]
      },
      { 
        id: "q13", 
        text: "What is the difference between useEffect and useLayoutEffect?",
        difficulty: "Medium",
        resources: [
          { title: "useEffect vs useLayoutEffect (Video)", url: "https://www.youtube.com/watch?v=w6a629S2B70", type: 'video' },
          { title: "useLayoutEffect (Docs)", url: "https://react.dev/reference/react/useLayoutEffect", type: 'docs' }
        ]
      },
      { 
        id: "q14", 
        text: "What are the rules of Hooks?",
        difficulty: "Easy",
        resources: [
          { title: "Rules of Hooks (Docs)", url: "https://react.dev/warnings/invalid-hook-call-warning", type: 'docs' }
        ]
      },
      { 
        id: "q15", 
        text: "How do you prevent unnecessary re-renders? (React.memo, useMemo, useCallback)",
        difficulty: "Medium",
        resources: [
          { title: "React Performance (Video)", url: "https://www.youtube.com/watch?v=uojLJFt9SzY", type: 'video' }
        ]
      },
      { 
        id: "q16", 
        text: "What is Prop Drilling, and how do you solve it?",
        difficulty: "Easy",
        resources: [
          { title: "Prop Drilling (Article)", url: "https://www.freecodecamp.org/news/prop-drilling-in-react-explained-with-examples/", type: 'article' }
        ]
      },
      { 
        id: "q17", 
        text: "Next.js: Explain CSR, SSR, SSG, and ISR.",
        difficulty: "Medium",
        resources: [
          { title: "Next.js Data Fetching (Video)", url: "https://www.youtube.com/watch?v=Sklc_fS8SBA", type: 'video' }
        ]
      },
      { 
        id: "q18", 
        text: "When would you choose Next.js over a standard React Vite app?",
        difficulty: "Easy",
        resources: [
          { title: "Next.js vs React (Video)", url: "https://www.youtube.com/watch?v=Sklc_fS8SBA", type: 'video' }
        ]
      },
      { 
        id: "q19", 
        text: "Next.js App Router: Server Component vs. Client Component?",
        difficulty: "Medium",
        resources: [
          { title: "RSC Explained (Video)", url: "https://www.youtube.com/watch?v=TQQPAU8S4mE", type: 'video' }
        ]
      },
      { 
        id: "q20", 
        text: "How does React Native differ from React.js under the hood?",
        difficulty: "Hard",
        resources: [
          { title: "React Native Architecture (Video)", url: "https://www.youtube.com/watch?v=0MlT74Zm60E", type: 'video' }
        ]
      },
      { 
        id: "q21", 
        text: "What is the difference between FlatList and ScrollView?",
        difficulty: "Easy",
        resources: [
          { title: "FlatList vs ScrollView (Video)", url: "https://www.youtube.com/watch?v=W-8_vD3G_0U", type: 'video' }
        ]
      },
    ],
  },
  {
    week: 3,
    title: "Node.js & Express.js",
    questions: [
      { 
        id: "q22", 
        text: "Explain the Node.js Event Loop. How does it handle concurrency?",
        difficulty: "Hard",
        resources: [
          { title: "Node.js Event Loop (Video)", url: "https://www.youtube.com/watch?v=PNa9OMajw9w", type: 'video' }
        ]
      },
      { 
        id: "q23", 
        text: "What is the difference between CommonJS and ES Modules?",
        difficulty: "Easy",
        resources: [
          { title: "CJS vs ESM (Video)", url: "https://www.youtube.com/watch?v=mK54Cn4ceac", type: 'video' }
        ]
      },
      { 
        id: "q24", 
        text: "What are Node.js Streams and when would you use them?",
        difficulty: "Hard",
        resources: [
          { title: "Node.js Streams (Video)", url: "https://www.youtube.com/watch?v=7S_txpZ_v_o", type: 'video' }
        ]
      },
      { 
        id: "q25", 
        text: "What is middleware in Express? Write a basic logging middleware.",
        difficulty: "Easy",
        resources: [
          { title: "Express Middleware (Video)", url: "https://www.youtube.com/watch?v=lY6icfhap2o", type: 'video' }
        ]
      },
      { 
        id: "q26", 
        text: "How do you handle asynchronous errors in Express?",
        difficulty: "Medium",
        resources: [
          { title: "Error Handling (Video)", url: "https://www.youtube.com/watch?v=H9M02of22z4", type: 'video' }
        ]
      },
      { 
        id: "q27", 
        text: "How would you structure a large Express application? (MVC, etc.)",
        difficulty: "Medium",
        resources: [
          { title: "Express Project Structure (Video)", url: "https://www.youtube.com/watch?v=vF7p_W40I_8", type: 'video' }
        ]
      },
      { 
        id: "q28", 
        text: "Explain the flow of JWT Authentication. How do you protect a route?",
        difficulty: "Medium",
        resources: [
          { title: "JWT Explained (Video)", url: "https://www.youtube.com/watch?v=7Q17ubgrkQs", type: 'video' }
        ]
      },
      { 
        id: "q29", 
        text: "What is CORS, and why do we need the cors package?",
        difficulty: "Easy",
        resources: [
          { title: "CORS Explained (Video)", url: "https://www.youtube.com/watch?v=4KHiSt0o_zE", type: 'video' }
        ]
      },
    ],
  },
  {
    week: 4,
    title: "Databases & Behavioral",
    questions: [
      { 
        id: "q30", 
        text: "MongoDB vs PostgreSQL: Primary differences and when to choose which?",
        difficulty: "Easy",
        resources: [
          { title: "SQL vs NoSQL (Video)", url: "https://www.youtube.com/watch?v=ZS_kXvOeQ5Y", type: 'video' }
        ]
      },
      { 
        id: "q31", 
        text: "PostgreSQL: What are JOINs? INNER JOIN vs LEFT JOIN.",
        difficulty: "Easy",
        resources: [
          { title: "SQL Joins (Video)", url: "https://www.youtube.com/watch?v=9yeOJ0ZMUYw", type: 'video' }
        ]
      },
      { 
        id: "q32", 
        text: "PostgreSQL: What are ACID properties?",
        difficulty: "Medium",
        resources: [
          { title: "ACID Explained (Video)", url: "https://www.youtube.com/watch?v=o7u_F13XU44", type: 'video' }
        ]
      },
      { 
        id: "q33", 
        text: "MongoDB: What is an aggregation pipeline?",
        difficulty: "Hard",
        resources: [
          { title: "Aggregation Pipeline (Video)", url: "https://www.youtube.com/watch?v=Kk6Er0c7srU", type: 'video' }
        ]
      },
      { 
        id: "q34", 
        text: "How do you handle relationships in MongoDB (References vs. Embedded)?",
        difficulty: "Medium",
        resources: [
          { title: "Data Modeling (Video)", url: "https://www.youtube.com/watch?v=leNCfU5SYR8", type: 'video' }
        ]
      },
      { 
        id: "q35", 
        text: "Behavioral: Hardest technical challenge you faced?",
        difficulty: "Medium",
        resources: [
          { title: "STAR Method (Video)", url: "https://www.youtube.com/watch?v=W7S_txpZ_v_o", type: 'video' }
        ]
      },
      { 
        id: "q36", 
        text: "Behavioral: A time you were stuck on a bug and how you fixed it?",
        difficulty: "Easy",
        resources: [
          { title: "Debugging Stories (Video)", url: "https://www.youtube.com/watch?v=Xp5K_v_p_v_o", type: 'video' }
        ]
      },
      { 
        id: "q37", 
        text: "Behavioral: How do you keep up with new technologies?",
        difficulty: "Easy",
        resources: [
          { title: "Staying Updated (Video)", url: "https://www.youtube.com/watch?v=Yp5K_v_p_v_o", type: 'video' }
        ]
      },
      { 
        id: "q38", 
        text: "Behavioral: How do you approach learning a completely new tech?",
        difficulty: "Easy",
        resources: [
          { title: "Learning New Tech (Video)", url: "https://www.youtube.com/watch?v=Zp5K_v_p_v_o", type: 'video' }
        ]
      },
    ],
  },
];

export const prepKit = {
  categories: [
    {
      title: "Core Fundamentals",
      resources: [
        { title: "JavaScript.info", url: "https://javascript.info/", type: 'docs' },
        { title: "MDN Web Docs", url: "https://developer.mozilla.org/en-US/", type: 'docs' }
      ]
    },
    {
      title: "DSA & Problem Solving",
      resources: [
        { title: "NeetCode 150", url: "https://neetcode.io/", type: 'tool' },
        { title: "Big-O Cheat Sheet", url: "https://www.bigocheatsheet.com/", type: 'docs' }
      ]
    }
  ],
  tips: [
    "DON'T GUESS: Explain your thought process if you don't know the answer.",
    "BIG-O: Always mention the time/space complexity of your solutions.",
    "TRADE-OFFS: Every tech choice has pros/cons. Articulate WHY you chose one tool over another."
  ]
};
```

## 4. Navigation Structure
- **Tab 1: CURRICULUM** (List view -> Question Detail with Resources & Interview Mode).
- **Tab 2: PREP KIT** (Study guide & Pro-tips).
- **Tab 3: PROGRESS** (Activity Heatmap & Reminders).

***

### Implementation Guidelines for the Developer:
1. **Brutalist UI**: Use absolute positioning and offset `View` components with black backgrounds to create the "Hard Shadow" effect. Standard React Native `elevation` or `shadow` props are often too soft for Brutalism.
2. **AI Integration**: Ensure the Gemini API key is handled via `process.env`. The review should return a JSON object with `rating` and `feedback`.
3. **Timer**: Use `setInterval` and manage the state carefully to prevent memory leaks. The pulse effect can be achieved using `Animated.loop` or `framer-motion-react-native`.
4. **Persistence**: Use Firebase Firestore to ensure user progress is synced across devices.
