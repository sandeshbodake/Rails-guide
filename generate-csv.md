```
require 'csv'

file = Rails.root.join('public/production_users_details.csv')

CSV.open(file, "wb") do |csv|
  csv << ["first_name", "last_name", "email", "user_type", "client", "roles"]
  User.all.each do |user|
    csv << [user.first_name, user.last_name, user.email, user.user_type, user.client.company_name, user.user_roles.map(&:name).join(",")]
  end
end
```
