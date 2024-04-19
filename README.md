# myBookShelf

Welcome to myBookShelf, an application designed to help users discover and explore books using the Google Books API. With myBookShelf, users can easily search for books and even have the option to purchase or download books that are available for free.

## Features

- Search for books using the Google Books API.
- View search results with pagination support.
- Purchase or download books directly from the application.
- Built with Vue.js and Bootstrap using Vite for efficient development.

#### Filtering

You can utilize the `Show Filters` button to refine your search results further. It accepts the following values:

- **partial**: Returns results where at least parts of the text are previewable.
- **full**: Only returns results where all of the text is viewable.
- **free-ebooks**: Only returns results that are free Google eBooks.
- **paid-ebooks**: Only returns results that are Google eBooks with a price.
- **ebooks**: Only returns results that are Google eBooks, whether paid or free. Non-eBooks such as publisher content available in limited preview or magazines are excluded.

#### Sorting

You can change the ordering by:

- **relevance**: Returns results in order of the relevance of search terms (this is the default).
- **newest**: Returns results in order of most recently to least recently published.


## Getting Started

To run the code locally, follow these steps:

1. Ensure you have [Node Version Manager (nvm)](https://github.com/nvm-sh/nvm) installed.
2. Run `nvm use` to use the appropriate Node.js version specified in the project.
3. Run `npm install` to install the dependencies.
4. Finally, run `npm run dev` to start the development server.

## Recommended Environment

- **IDE:** Visual Studio Code (VS Code)
- **Browser:** Google Chrome

## Known Issues

Please note that the pagination feature in myBookShelf may exhibit erratic behavior due to inconsistencies in the Google Books API response. At times, the API may return different result counts for the same query, leading to unexpected pagination behavior.

## Future Considerations

We are actively working on resolving the pagination issues to provide a smoother browsing experience for our users. Stay tuned for updates and improvements in future releases.

Thank you for using myBookShelf! Happy reading! 📚
