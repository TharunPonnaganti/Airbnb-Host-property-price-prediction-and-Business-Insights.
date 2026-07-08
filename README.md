# Airbnb Host Property Price Prediction and Business Insights

Comprehensive Airbnb New York City dataset with **106 features** and **285,619 listings** for property price prediction and business insights analysis.

## Dataset

- **Source:** [Kaggle - Airbnb New York City with 106 Features](https://www.kaggle.com/datasets/tharunponnaganti/airbnb-new-york-city-with-106-features)
- **File:** `airbnbmark1.csv` (~558 MB, stored via Git LFS)
- **Records:** 285,619 listings
- **Features:** 106 columns

### Feature Categories

| Category | Features |
|----------|----------|
| **Listing Info** | id, name, description, summary, space, notes, house_rules, property_type, room_type, amenities |
| **Host Details** | host_id, host_name, host_since, host_location, host_response_time/rate, host_is_superhost, host_verifications, host_listings_count |
| **Location** | street, neighbourhood, neighbourhood_cleansed, neighbourhood_group_cleansed, city, state, zipcode, latitude, longitude |
| **Property** | accommodates, bathrooms, bedrooms, beds, bed_type, square_feet |
| **Pricing** | price, weekly_price, monthly_price, security_deposit, cleaning_fee, extra_people, guests_included |
| **Availability** | availability_30/60/90/365, minimum_nights, maximum_nights, calendar_updated, has_availability |
| **Reviews** | number_of_reviews, review_scores_rating/accuracy/cleanliness/checkin/communication/location/value, reviews_per_month, first_review, last_review |
| **Policies** | cancellation_policy, instant_bookable, require_guest_profile_picture, require_guest_phone_verification |

## Setup

This repository uses [Git LFS](https://git-lfs.github.com/) for large files. After cloning:

```bash
git lfs install
git lfs pull
```
