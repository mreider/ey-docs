# Convert Capistrano Symlinks to AppCloud Symlinks

Many people use Capistrano to configure symlinks to shared folders.  Ideally you'd be able to fully deploy your app from the Engine Yard AppCloud UI without Capistrano.

## For Example

Here's an example of a common Capistrano recipes for symlinking folders:

<code ruby>
    after "deploy:update_code", "symlink_files"
    after "deploy:update_code", "symlink_geo_ip"

    task :symlink_files do
      %w(photo card).each do |folder|
        path  = "#{release_path}/../../shared/files/#{folder}"
        symlink_path = "#{release_path}/public/#{folder}"
        run "mkdir -p #{path}"
        puts "Symlinking #{folder} folder"
        run "rm -rf #{symlink_path}"
        run "ln -sf #{path} #{symlink_path}"
      end
    end
    
    task :symlink_geo_ip do
      path  = "#{release_path}/../../shared/GeoIPCity.dat"
      symlink_path = "#{release_path}/db/GeoIPCity.dat"
      puts "Symlinking GeoIP"
      run "rm -rf #{symlink_path}"
      run "ln -sf #{path} #{symlink_path}"
    end
</code>

## Using Deploy Hooks

Instead we suggest you use our new method of deploy hooks that will run the commands for you when you deploy your application using Engine Yard AppCloud's User Interface.

Create a file `$RAILS_ROOT/deploy/before_symlink.rb` with these contents:

<code ruby>
    %w(photo card).each do |folder|
        run "echo 'release_path: #{release_path}/public/#{folder}' >> #{shared_path}/logs.log"
        run "ln -nfs #{shared_path}/files/#{folder} #{release_path}/public/#{folder}"
    end

    run "echo 'release_path: #{release_path}/db/GeoIPCity.dat' >> #{shared_path}/logs.log"
    run "ln -nfs #{shared_path}/shared/GeoIPCity.dat #{release_path}/db/GeoIPCity.dat"
</code>

Please note we are using the "Release" path instead of "Current" as it hasn't been symlinked yet.

## Conclusion

By converting to deploy hooks, your app can now be deployed from the Engine Yard AppCloud UI without having to use Capistrano.
