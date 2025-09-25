# Contributing Experiments

Welcome to Bitpanda Labs! This directory contains our innovation experiments and prototypes. 

## How to Add a New Experiment

1. Create a folder with format `YYYY_MM_DD_experiment_name` and add a `index.md` file in this directory (the date 
doesn't have to reflect the publishing date, think of it as migration files structure, aka the date it was created, 
for sorting and easy find)
2. You can also add an `image.jpg` or `image.png` file for the card's header image. Otherwise, it'll take a default 
image from assets/img/image.png 
3. Use the template structure provided in `_template.md`
4. Fill in your experiment details following the Jekyll front matter format 
5. Submit a pull request with your experiment

## Front Matter Fields

- **layout**: Always use `experiment`
- **title**: A descriptive title for your experiment, stick to short titles that would fit in two lines
- **date**: Date in YYYY-MM-DD format (use future date for scheduled publishing)
- **categories**: Array of relevant categories (e.g., ["AI & Machine Learning", "User Experience"])
- **excerpt**: A brief intro that will appear on the homepage and before the main article, stick to 2–3 sentences 

## Scheduled Publishing

To schedule an experiment for future publication:
1. Set the `date` field to a future date (e.g., `2025-10-15`)
2. The experiment won't appear on the site until that date
3. The site automatically rebuilds daily at 9:00 AM UTC to publish scheduled content

## Content Structure

You can use any [Markdown](https://www.markdownguide.org/basic-syntax/) syntax. If it was used for the 
first time, it might be that it isn't styled yet, so please make sure it looks nice. You can refer to our 
[Brand Book](https://my.corebook.io/GG4F0OKXrjNAJt3Alto7wc8Qi9dbHOrF/basic-design-elements/typography)

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
- Company Culture

Feel free to create new categories that fit your experiment! 