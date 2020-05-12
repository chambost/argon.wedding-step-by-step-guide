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

# Step 1 - Basics

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

# Step 2 - Bootstrap

- `yarn add bootstrap jquery popper.js`

## config/webpack/environment.js

```
const webpack = require("webpack") 

environment.plugins.append(
    "Provide", 
    new webpack.ProvidePlugin({ 
        $: 'jquery',
        jQuery: 'jquery',
        Popper: ['popper.js', 'default']
})) 
```

# Step 3 - Identifier

- `rails g migration addIdentifierToVenue identifier:string`

## db/migrate/[timestamp]_add_identifer_to_venue.rb

```
    add_column :venues, :identifier, :string, null: false
```

## app/models/Venue.rb

```
    validates :mp_id, length: { is: 11 , message: "Matterport ID must be 11 alphanumeric characters" }
    validates_format_of :identifier, with: /\A[a-z0-9][a-z0-9\-]*\z/i, message: "Lowercase letters, digits and hyphens only in identifier"

```

## db/seeds.rb

```
Venue.create!([
    { mp_id: "fQTYVMprwRR" , identifier: "harry-potter"    , name: "Harry Potter Wedding" },
    { mp_id: "H7uhuanqpbM" , identifier: "rivergums"       , name: "Gordon Country - Rivergums Barn" },
    { mp_id: "3sBLzeJpKgG" , identifier: "red-barn"        , name: "Red Barn Wedding Venue" },
    { mp_id: "XX37AbvvVdy" , identifier: "vintage-gardens" , name: "Vintage Gardens Wedding Chapel" },
    { mp_id: "dJ525xBtLvo" , identifier: "silver-oaks"     , name: "Silver Oaks Chateau" },
    { mp_id: "nS8CpFAKGyH" , identifier: "turkish-palace"  , name: "Turkish Palace" },
    { mp_id: "ePPFeeNZ7sa" , identifier: "old-shire-hall"  , name: "The Old Shire Hall" },
    { mp_id: "httYSmxrvTT" , identifier: "brinkburn"       , name: "Brinkburn Northumberland" },
    { mp_id: "ZPaFvxZAL9u" , identifier: "broderie-room"   , name: "The Broderie Room at Phipps Conservatory" },
    { mp_id: "6prr3t2ySER" , identifier: "la-mar"          , name: "La Mar" },
    { mp_id: "aXdXVkktofc" , identifier: "the-lodge"       , name: "The Lodge at Grant's Trail" }
])
```

- https://guides.rubyonrails.org/routing.html#overriding-named-route-parameters

## config/routes.rb

```
  resources :venues, param: :identifier
```

## app/controllers/venues_controller.rb

```
    def set_venue
      @venue = Venue.find_by(identifier: params[:identifier])
    end

    # Only allow a list of trusted parameters through.
    def venue_params
      params.require(:venue).permit(:mp_id, :name, :identifier)
    end
```