# ROR (1) | collection  member route



```ruby
resources :photos do
  member do
    get :preview
  end
end
```



```ruby
resources :photos do
  collection do
    get :search
  end
end
```

```

                URL                 Helper                 
-----------------------------------------------------
member          /photos/1/preview   preview_photo_path(photo)  
collection      /photos/search      search_photos_path         
```

```
member: Acts on a specific resource so required id (preview specific photo)
collection:  Acts on collection of resources(display all photos)
```