<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  </head>
  <body>
    <h3>Step By Step Guid</h3>
    1. Create a project <br />
    <pre><code class="language-bash">
        composer create-project laravel/laravel laravel-multi-user-authentication
    </code></pre>
    <br />
    2. Open project in IDE and run the project <br />
    <pre><code class="language-bash">
        php artisan serve
    </code></pre>
    3. Connect database <br />
    <pre><code class="language-bash">
        DB_CONNECTION=mysql
        DB_HOST=127.0.0.1
        DB_PORT=3306
        DB_DATABASE=authentication
        DB_USERNAME=root
        DB_PASSWORD=
    </code></pre>
    4. Install <strong>Breeze</strong><br />
    <pre><code class="language-bash">
        run composer require laravel/breeze --dev
    </code></pre>
      <pre><code class="language-bash">
        php artisan breeze:install
    </code></pre>
      <pre><code class="language-bash">
        npm install
    </code></pre>
      <pre><code class="language-bash">
        npm run dev
    </code></pre>
    <br />
    5. Go to the <strong>migrations</strong> folder and add some items according
    to your needs in <strong>create_user_table.php</strong><br />
    <pre><code class="language-bash">
        public function up(): void
        {
            Schema::create('users', function (Blueprint $table) {
                $table->id();
                $table->string('name');
                $table->string('username')->nullable();
                $table->string('email')->unique();
                $table->timestamp('email_verified_at')->nullable();
                $table->string('password');
                $table->enum('role', ['admin', 'agent', 'user'])->default('user');
                $table->rememberToken();
                $table->timestamps();
            });
        }
    </code></pre>
    <br />
    6. Go to the <strong>Model</strong> folder and update
    <strong>User.php </strong><br />
    <pre><code class="language-bash">
        protected $guarded = [];
    </code></pre>
    <br />
    7. Migrate database <br />
    <pre><code class="language-bash">
        php artisan migrate
    </code></pre>
    <br />
    8. Create a new seeder named <strong>UserTableSeeder</strong><br />
    <pre><code class="language-bash">
        php artisan make:seeder UserTableSeeder 
    </code></pre>
    <br />
    9. Go to <strong>UserTableSeeder.php</strong> and add some value<br />
    <pre><code class="language-bash">
        public function run(): void
        {
            DB::table('users')->insert([
                // Admin
                [
                    'name' => 'Admin',
                    'username' => 'admin',
                    'email' => 'admin@gmail.com',
                    'password' => Hash::make('111'),
                    'role' => 'admin',
                ],
                // Agent
                [
                    // Agent Information
                ],
                // User
                [
                    // User Information
                ]
            ]);
        }
    </code></pre>
    10. Go to the <strong>factories</strong> folder and update the fake element
    in <strong>UserFactory.php</strong><br />
    <pre><code class="language-bash">
        public function definition(): array
        {
            return [
                'name' => fake()->name(),
                'email' => fake()->unique()->safeEmail(),
                'email_verified_at' => now(),
                'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password
                'role' => fake()->randomElement(['admin', 'agent', 'user']),
                'remember_token' => Str::random(10),
            ];
        }
    </code></pre>
    11. Go to <strong>DatabaseSeeder.php</strong> and update this so that
    <strong>UserTableSeeder.php</strong> has been called from this file <br />
    <pre><code class="language-bash">
        public function run(): void
        {
            $this->call(UserTableSeeder::class);
            \App\Models\User::factory(5)->create();
        }
    </code></pre>
    12. Migrate database <br />
    <pre><code class="language-bash">
        php artisan migrate:fresh --seed
    </code></pre>
    13. Go to <strong>web.php</strong> and make route for
    <strong>admin</strong> <br />
    <pre><code class="language-bash">
        Route::get('/admin/dashboard', [AdminController::class, 'AdminDashboard'])->name('admin.dashboard');
    </code></pre>
    14. Make a controller named <strong>AdminController</strong> <br />
    <pre><code class="language-bash">
        php artisan make:controller AdminController
    </code></pre>
    15. Go to <strong>AdminController.php</strong> and add a function named
    <strong>AdminDashboard</strong> <br />
    <pre><code class="language-bash">
        public function AdminDashboard(){   
        }
    </code></pre>
    16. Make a folder in <strong>view</strong> path named
    <strong>admin</strong> and make a file named
    <strong>admin_dashboard.blade.php</strong> in admin folder <br />
    17. Add some HTML code in <strong>admin_dashboard.blade.php</strong> <br />
    18. Go to <strong>AdminController.php</strong> and redirect
    <strong>AdminDashboard</strong> into
    <strong>admin.admin_dashboard</strong> file <br />
    <pre><code class="language-bash">
        public function AdminDashboard(){
            return view('admin.admin_dashboard');
        }
    </code></pre>
    19. Make a controller named AgentController <br />
    <pre><code class="language-bash">
        php artisan make:controller AgentController
    </code></pre>
    20. Go to <strong>web.php</strong> and make route for
    <strong>agent</strong> <br />
    <pre><code class="language-bash">
        Route::get('/agent/dashboard', [AgentController::class, 'AgentDashboard'])->name('agent.dashboard');
    </code></pre>
    21. Go to <strong>AgentController.php</strong> and add a function named
    <strong>AgentDashboard</strong> <br />
    <pre><code class="language-bash">
        public function AgentDashboard(){
        }
    </code></pre>
    22. Make a folder in <strong>view</strong> path named
    <strong>agent</strong> and make a file named
    <strong>agent_dashboard.blade.php</strong> in <strong>agent</strong> folder
    <br />
    23. Add some HTML code in <strong>agent_dashboard.blade.php</strong> <br />
    24. Go to <strong>AgentController.php</strong> and redirect
    <strong>AgentDashboard</strong> into
    <strong>agent.agent_dashboard</strong> file <br />
    <pre><code class="language-bash">
        public function AdminDashboard(){
            return view('admin.admin_dashboard');
        }
    </code></pre>
    25. Go to <strong>Auth</strong> file and open
    <strong>AuthenticatedSessionController.php</strong> and update it so that
    after login, user has been redirecting to the appropriate page
    <br />
    <pre><code class="language-bash">
        public function store(LoginRequest $request): RedirectResponse
        {
            $request->authenticate();
            $request->session()->regenerate();
            $url = '';
            if($request->user() -> role === 'admin'){
                $url = 'admin/dashboard';
            } elseif($request -> user() -> role === 'agent'){
                $url = 'agent/dashboard';
            } elseif($request -> user() -> role === 'user'){
                $url = '/dashboard';
            }
            return redirect()->intended($url);
        }
    </code></pre>
    25. Make middleware named <strong>Role</strong> <br />
    <pre><code class="language-bash">
        php artisan make:middleware Role
    </code></pre>
    27. Register <strong>Role</strong> middleware into
    <strong>$middlewareAliases</strong> in the
    <strong>Kernel.php</strong> file<br />
    <pre><code class="language-bash">
        'role' => \App\Http\Middleware\Role::class,
    </code></pre>
    28. Go to <strong>Role</strong> middleware, add <strong>$role</strong> value
    in <strong>handle function</strong> and update that function <br />
    <pre><code class="language-bash">
        public function handle(Request $request, Closure $next, $role): Response
        {
            if($request->user()-> role !== $role){
                return redirect('dashboard');
            }
            return $next($request);
        }
    </code></pre>
    29. Go to <strong>web.php</strong> and protect
    <strong>admin route</strong> and <strong>agent route</strong> through
    middleware
    <br />
    <pre><code class="language-bash">
        // Admin Route
        Route::middleware(['auth', 'role:admin'])->group(function(){
            Route::get('/admin/dashboard', [AdminController::class, 'AdminDashboard'])->name('admin.dashboard');
        });
        // Agent Route
        Route::middleware(['auth', 'role:agent'])->group(function(){
            Route::get('/agent/dashboard', [AgentController::class, 'AgentDashboard'])->name('agent.dashboard');     
        });
    </code></pre>
  </body>
</html>
