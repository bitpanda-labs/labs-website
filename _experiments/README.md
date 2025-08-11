# Contributing Experiments

Welcome to Bitpanda Labs! This directory contains our innovation experiments and prototypes. 

## How to Add a New Experiment

1. Create a folder with format `YYYY_MM_DD_experiment_name` and add a `index.md` file in this directory
2. You can also add an `image.jpg` or `image.png` file for the card's header image. Otherwise it'll take a default image from assets/img/image.png 
3. Use the template structure provided in `_template.md`
4. Fill in your experiment details following the Jekyll front matter format 
5. Submit a pull request with your experiment

## Front Matter Fields

- **layout**: Always use `experiment`
- **title**: A descriptive title for your experiment
- **date**: Date in YYYY-MM-DD format
- **categories**: Array of relevant categories (e.g., ["AI & Machine Learning", "User Experience"])
- **excerpt**: A brief summary that will appear on the homepage

## Content Structure

Your experiment should include:
- **The Challenge/Idea**: What problem were you solving?
- **The Approach/Experiment**: How did you tackle it?
- **The Outcome/Results**: What did you learn or achieve?
- **Next Steps**: Where is this heading?

## Categories

Common categories include:
- AI & Machine Learning
- User Experience
- Blockchain & DeFi
- Payment Innovation
- Security & Privacy
- Data Analytics
- Mobile Development
- API Development

Feel free to create new categories that fit your experiment! 