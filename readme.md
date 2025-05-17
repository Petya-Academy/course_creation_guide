# Course Creation Guide for Academy Platform

## Overview

This guide provides a comprehensive tutorial for course creators on how to structure and implement courses using our JSON format. This is a public guide available to anyone interested in creating courses for our academy platform.

> **Important Notice**: You can test your course on our public testing endpoint at https://petya.pw/academy/course/demo. However, to officially publish your course, please contact us at [academy@petya.pw](mailto:academy@petya.pw) or reach out on our [Discord server](https://discord.gg/jCfJZ5eKvN) for more information.

## Table of Contents

1. [Course Structure](#course-structure)
2. [Defining a Course](#defining-a-course)
3. [Creating Modules](#creating-modules)
4. [Adding Pages](#adding-pages)
5. [Creating Exercises](#creating-exercises)
6. [Validation Scripts](#validation-scripts)
7. [Best Practices](#best-practices)

## Course Structure

The course data follows this JSON structure:

```json
{
  "id": "string", // Unique identifier (kebab-case)
  "title": "string", // Display name
  "description": "string", // Brief description (optional)
  "category": "string", // e.g., "System Admin"
  "level": "string", // e.g., "Beginner"
  "image": "string", // Optional header image
  "icon": "string", // Emoji or icon (optional)
  "thumbnail": "string", // Thumbnail image path (optional)
  "modules": [], // Array of course modules
  "isHidden": false // Optional visibility flag
}
```

## Defining a Course

1. **Basic Course Setup**
   ```json
   {
     "id": "your-course-id", // Must be unique, kebab-case
     "title": "Your Course Title",
     "description": "A brief description of your course",
     "category": "Category Name",
     "level": "Beginner/Intermediate/Advanced",
     "icon": "🎯", // Optional emoji
     "thumbnail": "/thumbnails/your-course.jpg", // Optional
     "modules": [
       /* Modules go here */
     ]
   }
   ```

## Creating Modules

Each course contains one or more modules:

```json
{
  "id": "string", // Unique within course
  "title": "string", // Display name
  "description": "string", // Optional description
  "thumbnail": "string", // Optional thumbnail
  "pages": [] // Array of pages
}

// Example module
{
  "id": "module-1",
  "title": "Introduction to Topic",
  "description": "Learn the basics of...",
  "thumbnail": "/thumbnails/module1.jpg",
  "pages": []
}
```

## Adding Pages

Each module contains multiple pages:

```json
{
  "id": "string", // Unique within module
  "title": "string", // Display title
  "content": "string", // Markdown content
  "hasExercise": false, // Whether this page has an exercise
  "exercise": {} // Optional exercise object
}

// Example page
{
  "id": "page-1",
  "title": "Getting Started",
  "content": "# Welcome!\n\nThis is a markdown content...",
  "hasExercise": false
}
```

## Creating Exercises

For pages with exercises, add an exercise object:

```json
{
  "id": "string", // Unique within course
  "instructions": "string", // What the user needs to do
  "hint": "string", // Optional hint
  "solution": "string", // Expected command/input
  "validationScript": "string" // JavaScript validation function
}

// Example exercise
{
  "id": "hello-world-exercise",
  "instructions": "Type 'echo Hello World' in the terminal.",
  "hint": "Use the echo command followed by text in quotes.",
  "solution": "echo Hello World",
  "validationScript": "function validateSolution(input) {\n  const command = input.trim();\n  if (command === \"echo Hello World\") {\n    return {\n      success: true,\n      output: \"Hello World\"\n    };\n  }\n  return {\n    success: false,\n    output: \"Incorrect command. Try again.\"\n  };\n}"
}
```

## Validation Scripts

Validation scripts are JavaScript functions that:

- Take user input as a parameter
- Return an object with `success` (boolean) and `output` (string) properties
- Can perform complex validation logic

Example validation script that checks for a specific command:

```javascript
function validateSolution(input) {
  const command = input.trim();
  if (command === "ls -l") {
    return {
      success: true,
      output: "total 0\n-rw-r--r-- 1 user user 0 Jan 1 00:00 file.txt",
    };
  }
  return {
    success: false,
    output: "Try using the ls command with the -l flag.",
  };
}
```

## Best Practices

1. **IDs**: Use kebab-case and make them descriptive (e.g., `file-operations`)
2. **Content**: Use Markdown for rich formatting in page content
3. **Exercises**:
   - Keep instructions clear and concise
   - Provide helpful error messages
   - Test all exercises thoroughly
4. **Validation Scripts**:
   - Handle edge cases
   - Be specific in error messages
   - Return realistic output for successful commands
5. **Organization**:
   - Group related content into modules
   - Order pages logically
   - Use thumbnails consistently

## Submitting Your Course

To submit your course for review and potential publication:

1. Prepare your course content following the structure outlined in this guide
2. Test your course locally if you have access to the development environment
3. Contact us at [academy@petya.pw](mailto:academy@petya.pw) with:
   - Your course content
   - A brief description of the course
   - Your contact information
   - Any questions you might have

Alternatively, you can reach out to us on our [Discord server](https://petya.pw) to discuss your course idea before submission.

## Example Complete Course

```json
{
  "id": "my-awesome-course",
  "title": "My Awesome Course",
  "description": "Learn amazing things with this course",
  "category": "Tutorials",
  "level": "Beginner",
  "icon": "🚀",
  "modules": [
    {
      "id": "getting-started",
      "title": "Getting Started",
      "pages": [
        {
          "id": "welcome",
          "title": "Welcome",
          "content": "# Welcome!\n\nThis is the first page of the course.",
          "hasExercise": false
        },
        {
          "id": "first-command",
          "title": "Your First Command",
          "content": "# Try This Command\n\nType 'pwd' to see your current directory.",
          "hasExercise": true,
          "exercise": {
            "id": "pwd-exercise",
            "instructions": "Type 'pwd' to print the working directory",
            "hint": "Just type 'pwd' and press Enter",
            "solution": "pwd",
            "validationScript": "function validateSolution(input) {\n                if (input.trim() === 'pwd') {\n                  return {\n                    success: true,\n                    output: '/home/user'\n                  };\n                }\n                return {\n                  success: false,\n                  output: 'Try typing just: pwd'\n                };\n              }"
          }
        }
      ]
    }
  ]
}
```

## Testing Your Course

You can now test your course directly in our demo environment!

1. Visit [https://petya.pw/academy/course/demo](https://petya.pw/academy/course/demo)
2. Paste your course JSON into the editor
3. Click "Preview Course" to see how your course will look and function
4. Test navigation between pages and modules
5. Try out interactive exercises to ensure they work correctly

The demo environment provides a full preview of your course including:

- All content rendering with proper markdown formatting
- Working interactive terminal for exercises
- Module and page navigation
- Visual styling exactly as it will appear when published

This allows you to verify your course works correctly before submitting it to us.

## Troubleshooting

- **Course not showing up?** Check if `isHidden` is set to `false` or undefined
- **Exercise not working?** Verify the validation script syntax and logic
- **Markdown not rendering?** Check for unescaped special characters

## Submission Process

After testing your course in our demo environment and confirming everything works correctly:

1. **Save your course JSON** - Keep a backup of your validated course JSON
2. **Contact us** - Reach out via one of these methods:
   - Email: [academy@petya.pw](mailto:academy@petya.pw)
   - Discord: [Join our server](https://discord.gg/jCfJZ5eKvN)
3. **Attach your course JSON** - Include your course data with your submission
4. **Provide any additional context** - Let us know about your course goals and target audience

Our team will review your submission and contact you regarding the next steps for official publication.
