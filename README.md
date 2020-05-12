- `rails new argon --database=postgresql`
- `git add .`
- `git commit -m "Initial commit"`
- `rails db:setup`
- `rails db:migrate`
- `heroku create argon-wedding`
- `git push heroku master`
- `rails g controller home index`
- `heroku domains:add argon.wedding`
- `heroku domains`
- `rails g scaffold venue mp_id:string name:string`
- `rails db:migrate`

# Step 1

## config/routes.db

```
  root 'venues#index'
```

## app/views/layouts/application.html.erb

```
    <title>Argon Wedding</title>
```

## app/models/venue.rb

```
    validates :mp_id, length: { is: 11 }
```

## db.seeds

```
Venue.create!([
    { mp_id: "fQTYVMprwRR" , name: "Harry Potter Wedding" },
    { mp_id: "H7uhuanqpbM" , name: "Gordon Country - Rivergums Barn" },
    { mp_id: "3sBLzeJpKgG" , name: "Red Barn Wedding Venue" },
    { mp_id: "XX37AbvvVdy" , name: "Vintage Gardens Wedding Chapel" },
    { mp_id: "dJ525xBtLvo" , name: "Silver Oaks Chateau" },
    { mp_id: "nS8CpFAKGyH" , name: "Turkish Palace" },
    { mp_id: "ePPFeeNZ7sa" , name: "The Old Shire Hall" },
    { mp_id: "httYSmxrvTT" , name: "Brinkburn Northumberland" },
    { mp_id: "ZPaFvxZAL9u" , name: "The Broderie Room at Phipps Conservatory" },
    { mp_id: "6prr3t2ySER" , name: "La Mar" },
    { mp_id: "aXdXVkktofc" , name: "The Lodge at Grant's Trail" }
])
```

## app/view/venues/index.html.erb

```
        <td>
          <iframe width="853" height="480" src="https://my.matterport.com/show/?m=<%= venue.mp_id %>" frameborder="0" allowfullscreen allow="vr">
          </iframe>
        </td>
```