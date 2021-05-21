# Audible Web Scraping

### Background
Audible is a audiobook (and podcast) distribution company founded in 1995 and acuqired by Amazon in 2008. The company sells mostly audiobooks using a mix of subscription (1 book / month) or a la carte pricing which tends to be higher. They offer audio media covering every genre, length, and type, and act as a distributor and producer for the content. Recent innovations include further integration with the Kindle and other amazon devices, having a-list actors record books, and producing original content. 

### Requirements
Please make sure the **requirements.txt** has been met before running 

### Web Scraper
Two Scrapy spiders were used to scrape the category structure and title information. Using the [category search page](https://www.audible.com/categories) the top level categories were scraped recursively by the 'category_spider' spider. At each category page [(example here)](https://www.audible.com/cat/Science-Fiction/Cyberpunk-Audiobooks/18580633011?ref=a_cat_Scien_c0_subCat_16&pf_rd_p=4fdf5dba-0b06-48f5-a441-a92b57cca04f&pf_rd_r=X3E9WTYNJSJQCQRN7QRX) the current category name was recorded along with the link to the best sellers of the category and the number of titles in the category. The super-category (one level up) was recorded along with a boolean indicating if the current category is the final path, indicated by bolded text in the sub-category area. If there was not a bolded category the spider passes the url of each sub-category to the same method recursively. 

There was a minor issues with 302 response codes, generally these shifted from one branch to another so:
- "Childrens / Action-Adventure" redirects to "Literature-Fiction / Action-Adventure"
- "Fairy-Tales-Folk-Tales-Myths / Adaptations" redirects to "Fantasy-Magic / Fairy-Tale-Adaptations"
- "Biographies / Historical" redirects to "History / Historical-Biographies"

Since the category structure was to be analyzed sperately, this data was stored in a CSV file and another spider scraped the title information. The 'title_spider' takes the best seller (or in two instances the 'See all in Category') [page](https://www.audible.com/search?node=18580638011&searchRank=salesrank&ref=a_cat_Scien_c4_showmore&pf_rd_p=edab67c5-fa72-4f54-8432-47fd8ef798c3&pf_rd_r=AEYZ7KCVH6FS4X08TWS2) and passes all title cards to another parse method. At the end of the page, the next page is passed recursively. Luckily the audible title cards contain all information we set out to scrape, title and subtitle, author(s) and narrator(s), length and language, price, ratings, and release date. 

### Data Analysis
Included in this repo is a jupyter notebook with my data analysis and some suggestions to get you started. While you can load the notebook to check out the exact findings, I look at the category sturcture in depth, analyze category abundance across time, and highlight some interesting titles; such as one that sells for $115 with 5-stars.




### Note
This project originally attempted to scraped Audible and Amazon for information and combine the data sets. Due to anti-bot measures used on Amazon.com, only 500 urls can be visited before a CAPTCHA is displayed. A solution using a proxy service was found but the project was forked into a seperate [repo found here](https://github.com/jwelch1123/amazon_scrape.git).

