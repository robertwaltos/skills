---
name: mobile-development
description: Expert-level mobile application development - iOS, Android, cross-platform frameworks, performance optimization, and App Store best practices
keywords: [mobile, ios, android, swift, kotlin, react-native, flutter, swiftui, jetpack-compose, app-store, mobile-performance, push-notifications]
use_cases:
  - Native iOS development (Swift, SwiftUI)
  - Native Android development (Kotlin, Jetpack Compose)
  - Cross-platform development (React Native, Flutter)
  - Mobile architecture patterns (MVVM, Clean Architecture, MVI)
  - Mobile performance optimization
  - App Store and Play Store submission
  - Push notifications and background processing
  - Mobile CI/CD and testing
license: MIT
---

# Mobile Development Skill

> **Expertise Level**: Staff/Principal Mobile Engineer - Equivalent to 15+ years iOS and Android development experience

## Purpose

Transform mobile application requirements into production-quality native and cross-platform applications. This skill provides expert-level guidance for architecture, performance, platform-specific best practices, and successful app store deployment.

## Related Skills

- **frontend-design** - UI/UX patterns applicable to mobile
- **performance-optimization** - Mobile-specific performance tuning
- **api-design** - Backend API design for mobile clients
- **security-audit** - Mobile security best practices
- **user-research** - Mobile user experience research

---

## Core Process

### Phase 1: Platform Strategy Assessment

```markdown
## Mobile Platform Decision Matrix

### Requirements Analysis
| Requirement | Weight | iOS Native | Android Native | React Native | Flutter |
|-------------|--------|------------|----------------|--------------|---------|
| Performance critical | [1-5] | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| Time to market | [1-5] | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Platform-specific features | [1-5] | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| Team expertise | [varies] | [assess] | [assess] | [assess] | [assess] |
| Long-term maintenance | [1-5] | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| Code sharing | [1-5] | ⭐ | ⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

### Platform Selection Criteria
- **Choose Native when**: Performance-critical, platform-specific features, large team
- **Choose Cross-platform when**: MVP, small team, content-focused apps, rapid iteration
- **Choose Flutter when**: Custom UI required, Dart expertise, Google ecosystem
- **Choose React Native when**: Web team background, JavaScript ecosystem, rapid prototyping
```

### Phase 2: iOS Development (SwiftUI)

```swift
// MARK: - Clean Architecture with SwiftUI

// Domain Layer - Entity
struct User: Identifiable, Codable, Equatable {
    let id: UUID
    let name: String
    let email: String
    let avatarURL: URL?
    let createdAt: Date
}

// Domain Layer - Repository Protocol
protocol UserRepository {
    func fetchUser(id: UUID) async throws -> User
    func fetchUsers() async throws -> [User]
    func updateUser(_ user: User) async throws -> User
    func deleteUser(id: UUID) async throws
}

// Domain Layer - Use Case
protocol FetchUsersUseCase {
    func execute() async throws -> [User]
}

final class FetchUsersUseCaseImpl: FetchUsersUseCase {
    private let repository: UserRepository
    
    init(repository: UserRepository) {
        self.repository = repository
    }
    
    func execute() async throws -> [User] {
        try await repository.fetchUsers()
    }
}

// Data Layer - API Service
final class APIService {
    private let session: URLSession
    private let baseURL: URL
    private let decoder: JSONDecoder
    
    init(baseURL: URL, session: URLSession = .shared) {
        self.baseURL = baseURL
        self.session = session
        self.decoder = JSONDecoder()
        self.decoder.dateDecodingStrategy = .iso8601
    }
    
    func request<T: Decodable>(_ endpoint: Endpoint) async throws -> T {
        let request = try endpoint.urlRequest(baseURL: baseURL)
        
        let (data, response) = try await session.data(for: request)
        
        guard let httpResponse = response as? HTTPURLResponse else {
            throw APIError.invalidResponse
        }
        
        guard (200...299).contains(httpResponse.statusCode) else {
            throw APIError.httpError(statusCode: httpResponse.statusCode)
        }
        
        return try decoder.decode(T.self, from: data)
    }
}

// Data Layer - Repository Implementation
final class UserRepositoryImpl: UserRepository {
    private let apiService: APIService
    private let cache: UserCache
    
    init(apiService: APIService, cache: UserCache) {
        self.apiService = apiService
        self.cache = cache
    }
    
    func fetchUsers() async throws -> [User] {
        // Check cache first
        if let cached = await cache.getUsers(), !cached.isEmpty {
            // Return cached and refresh in background
            Task { try? await refreshUsers() }
            return cached
        }
        
        return try await refreshUsers()
    }
    
    @discardableResult
    private func refreshUsers() async throws -> [User] {
        let users: [User] = try await apiService.request(.users)
        await cache.setUsers(users)
        return users
    }
}

// Presentation Layer - ViewModel
@MainActor
final class UsersViewModel: ObservableObject {
    @Published private(set) var users: [User] = []
    @Published private(set) var isLoading = false
    @Published private(set) var error: Error?
    
    private let fetchUsersUseCase: FetchUsersUseCase
    
    init(fetchUsersUseCase: FetchUsersUseCase) {
        self.fetchUsersUseCase = fetchUsersUseCase
    }
    
    func loadUsers() async {
        isLoading = true
        error = nil
        
        do {
            users = try await fetchUsersUseCase.execute()
        } catch {
            self.error = error
        }
        
        isLoading = false
    }
}

// Presentation Layer - SwiftUI View
struct UsersView: View {
    @StateObject private var viewModel: UsersViewModel
    
    init(viewModel: UsersViewModel) {
        _viewModel = StateObject(wrappedValue: viewModel)
    }
    
    var body: some View {
        NavigationStack {
            Group {
                if viewModel.isLoading && viewModel.users.isEmpty {
                    ProgressView()
                } else if let error = viewModel.error, viewModel.users.isEmpty {
                    ErrorView(error: error) {
                        Task { await viewModel.loadUsers() }
                    }
                } else {
                    userList
                }
            }
            .navigationTitle("Users")
            .refreshable {
                await viewModel.loadUsers()
            }
        }
        .task {
            await viewModel.loadUsers()
        }
    }
    
    private var userList: some View {
        List(viewModel.users) { user in
            NavigationLink(value: user) {
                UserRow(user: user)
            }
        }
        .navigationDestination(for: User.self) { user in
            UserDetailView(user: user)
        }
    }
}

// MARK: - Dependency Injection Container
@MainActor
final class DependencyContainer {
    static let shared = DependencyContainer()
    
    private init() {}
    
    // Services
    lazy var apiService: APIService = {
        APIService(baseURL: URL(string: "https://api.example.com")!)
    }()
    
    lazy var userCache: UserCache = {
        UserCacheImpl()
    }()
    
    // Repositories
    lazy var userRepository: UserRepository = {
        UserRepositoryImpl(apiService: apiService, cache: userCache)
    }()
    
    // Use Cases
    func makeFetchUsersUseCase() -> FetchUsersUseCase {
        FetchUsersUseCaseImpl(repository: userRepository)
    }
    
    // ViewModels
    func makeUsersViewModel() -> UsersViewModel {
        UsersViewModel(fetchUsersUseCase: makeFetchUsersUseCase())
    }
}
```

### Phase 3: Android Development (Kotlin + Jetpack Compose)

```kotlin
// Clean Architecture with Jetpack Compose

// Domain Layer - Entity
data class User(
    val id: String,
    val name: String,
    val email: String,
    val avatarUrl: String?,
    val createdAt: Instant
)

// Domain Layer - Repository
interface UserRepository {
    fun getUsers(): Flow<Result<List<User>>>
    suspend fun getUser(id: String): Result<User>
    suspend fun updateUser(user: User): Result<User>
    suspend fun deleteUser(id: String): Result<Unit>
}

// Domain Layer - Use Case
class GetUsersUseCase @Inject constructor(
    private val repository: UserRepository
) {
    operator fun invoke(): Flow<Result<List<User>>> = repository.getUsers()
}

// Data Layer - API
interface UserApi {
    @GET("users")
    suspend fun getUsers(): List<UserDto>
    
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): UserDto
    
    @PUT("users/{id}")
    suspend fun updateUser(
        @Path("id") id: String,
        @Body user: UserDto
    ): UserDto
}

// Data Layer - DTO with Mapping
@Serializable
data class UserDto(
    val id: String,
    val name: String,
    val email: String,
    @SerialName("avatar_url") val avatarUrl: String?,
    @SerialName("created_at") val createdAt: String
) {
    fun toDomain(): User = User(
        id = id,
        name = name,
        email = email,
        avatarUrl = avatarUrl,
        createdAt = Instant.parse(createdAt)
    )
}

// Data Layer - Repository Implementation
class UserRepositoryImpl @Inject constructor(
    private val api: UserApi,
    private val userDao: UserDao,
    private val dispatcher: CoroutineDispatcher = Dispatchers.IO
) : UserRepository {
    
    override fun getUsers(): Flow<Result<List<User>>> = flow {
        // Emit cached data first
        userDao.getAllUsers()
            .map { entities -> entities.map { it.toDomain() } }
            .collect { cachedUsers ->
                if (cachedUsers.isNotEmpty()) {
                    emit(Result.success(cachedUsers))
                }
            }
        
        // Fetch from network
        try {
            val users = api.getUsers().map { it.toDomain() }
            userDao.insertAll(users.map { it.toEntity() })
            emit(Result.success(users))
        } catch (e: Exception) {
            emit(Result.failure(e))
        }
    }.flowOn(dispatcher)
}

// Presentation Layer - UI State
data class UsersUiState(
    val users: List<User> = emptyList(),
    val isLoading: Boolean = false,
    val error: String? = null
)

sealed interface UsersEvent {
    data object Refresh : UsersEvent
    data class UserClicked(val user: User) : UsersEvent
    data object ErrorDismissed : UsersEvent
}

sealed interface UsersEffect {
    data class NavigateToUser(val userId: String) : UsersEffect
    data class ShowSnackbar(val message: String) : UsersEffect
}

// Presentation Layer - ViewModel
@HiltViewModel
class UsersViewModel @Inject constructor(
    private val getUsersUseCase: GetUsersUseCase
) : ViewModel() {
    
    private val _uiState = MutableStateFlow(UsersUiState())
    val uiState: StateFlow<UsersUiState> = _uiState.asStateFlow()
    
    private val _effects = Channel<UsersEffect>()
    val effects: Flow<UsersEffect> = _effects.receiveAsFlow()
    
    init {
        loadUsers()
    }
    
    fun onEvent(event: UsersEvent) {
        when (event) {
            is UsersEvent.Refresh -> loadUsers()
            is UsersEvent.UserClicked -> {
                viewModelScope.launch {
                    _effects.send(UsersEffect.NavigateToUser(event.user.id))
                }
            }
            is UsersEvent.ErrorDismissed -> {
                _uiState.update { it.copy(error = null) }
            }
        }
    }
    
    private fun loadUsers() {
        viewModelScope.launch {
            _uiState.update { it.copy(isLoading = true) }
            
            getUsersUseCase()
                .collect { result ->
                    result.fold(
                        onSuccess = { users ->
                            _uiState.update {
                                it.copy(users = users, isLoading = false, error = null)
                            }
                        },
                        onFailure = { error ->
                            _uiState.update {
                                it.copy(isLoading = false, error = error.message)
                            }
                        }
                    )
                }
        }
    }
}

// Presentation Layer - Compose UI
@Composable
fun UsersScreen(
    viewModel: UsersViewModel = hiltViewModel(),
    onNavigateToUser: (String) -> Unit
) {
    val uiState by viewModel.uiState.collectAsStateWithLifecycle()
    val snackbarHostState = remember { SnackbarHostState() }
    
    LaunchedEffect(Unit) {
        viewModel.effects.collect { effect ->
            when (effect) {
                is UsersEffect.NavigateToUser -> onNavigateToUser(effect.userId)
                is UsersEffect.ShowSnackbar -> {
                    snackbarHostState.showSnackbar(effect.message)
                }
            }
        }
    }
    
    Scaffold(
        topBar = {
            TopAppBar(title = { Text("Users") })
        },
        snackbarHost = { SnackbarHost(snackbarHostState) }
    ) { paddingValues ->
        Box(
            modifier = Modifier
                .fillMaxSize()
                .padding(paddingValues)
        ) {
            when {
                uiState.isLoading && uiState.users.isEmpty() -> {
                    CircularProgressIndicator(
                        modifier = Modifier.align(Alignment.Center)
                    )
                }
                uiState.error != null && uiState.users.isEmpty() -> {
                    ErrorContent(
                        error = uiState.error!!,
                        onRetry = { viewModel.onEvent(UsersEvent.Refresh) },
                        modifier = Modifier.align(Alignment.Center)
                    )
                }
                else -> {
                    UsersList(
                        users = uiState.users,
                        onUserClick = { user ->
                            viewModel.onEvent(UsersEvent.UserClicked(user))
                        },
                        onRefresh = { viewModel.onEvent(UsersEvent.Refresh) },
                        isRefreshing = uiState.isLoading
                    )
                }
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
private fun UsersList(
    users: List<User>,
    onUserClick: (User) -> Unit,
    onRefresh: () -> Unit,
    isRefreshing: Boolean,
    modifier: Modifier = Modifier
) {
    val pullRefreshState = rememberPullToRefreshState()
    
    PullToRefreshBox(
        isRefreshing = isRefreshing,
        onRefresh = onRefresh,
        state = pullRefreshState,
        modifier = modifier
    ) {
        LazyColumn(
            contentPadding = PaddingValues(16.dp),
            verticalArrangement = Arrangement.spacedBy(8.dp)
        ) {
            items(users, key = { it.id }) { user ->
                UserCard(
                    user = user,
                    onClick = { onUserClick(user) }
                )
            }
        }
    }
}
```

### Phase 4: React Native (TypeScript)

```typescript
// Clean Architecture with React Native + TypeScript

// Domain Layer - Entity
interface User {
  id: string;
  name: string;
  email: string;
  avatarUrl?: string;
  createdAt: Date;
}

// Domain Layer - Repository Interface
interface UserRepository {
  getUsers(): Promise<User[]>;
  getUser(id: string): Promise<User>;
  updateUser(user: User): Promise<User>;
  deleteUser(id: string): Promise<void>;
}

// Data Layer - API Service
class ApiService {
  private baseUrl: string;

  constructor(baseUrl: string) {
    this.baseUrl = baseUrl;
  }

  async request<T>(endpoint: string, options?: RequestInit): Promise<T> {
    const response = await fetch(`${this.baseUrl}${endpoint}`, {
      ...options,
      headers: {
        'Content-Type': 'application/json',
        ...options?.headers,
      },
    });

    if (!response.ok) {
      throw new ApiError(response.status, await response.text());
    }

    return response.json();
  }
}

// Data Layer - Repository Implementation
class UserRepositoryImpl implements UserRepository {
  constructor(
    private api: ApiService,
    private storage: AsyncStorage
  ) {}

  async getUsers(): Promise<User[]> {
    // Check cache first
    const cached = await this.storage.getItem('users');
    if (cached) {
      // Refresh in background
      this.refreshUsers();
      return JSON.parse(cached);
    }

    return this.refreshUsers();
  }

  private async refreshUsers(): Promise<User[]> {
    const users = await this.api.request<UserDto[]>('/users');
    const mapped = users.map(this.mapToDomain);
    await this.storage.setItem('users', JSON.stringify(mapped));
    return mapped;
  }

  private mapToDomain(dto: UserDto): User {
    return {
      id: dto.id,
      name: dto.name,
      email: dto.email,
      avatarUrl: dto.avatar_url,
      createdAt: new Date(dto.created_at),
    };
  }
}

// Presentation Layer - Custom Hook with React Query
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';

export function useUsers() {
  const queryClient = useQueryClient();
  const repository = useDependency<UserRepository>('UserRepository');

  const query = useQuery({
    queryKey: ['users'],
    queryFn: () => repository.getUsers(),
    staleTime: 5 * 60 * 1000, // 5 minutes
  });

  const deleteMutation = useMutation({
    mutationFn: (id: string) => repository.deleteUser(id),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
  });

  return {
    users: query.data ?? [],
    isLoading: query.isLoading,
    isRefreshing: query.isRefetching,
    error: query.error,
    refetch: query.refetch,
    deleteUser: deleteMutation.mutate,
    isDeleting: deleteMutation.isPending,
  };
}

// Presentation Layer - Screen Component
import React, { useCallback } from 'react';
import {
  FlatList,
  RefreshControl,
  StyleSheet,
  View,
  ActivityIndicator,
} from 'react-native';
import { SafeAreaView } from 'react-native-safe-area-context';

export function UsersScreen({ navigation }) {
  const {
    users,
    isLoading,
    isRefreshing,
    error,
    refetch,
    deleteUser,
  } = useUsers();

  const handleUserPress = useCallback((user: User) => {
    navigation.navigate('UserDetail', { userId: user.id });
  }, [navigation]);

  const renderItem = useCallback(({ item }: { item: User }) => (
    <UserCard
      user={item}
      onPress={() => handleUserPress(item)}
      onDelete={() => deleteUser(item.id)}
    />
  ), [handleUserPress, deleteUser]);

  const keyExtractor = useCallback((item: User) => item.id, []);

  if (isLoading && users.length === 0) {
    return (
      <View style={styles.centered}>
        <ActivityIndicator size="large" />
      </View>
    );
  }

  if (error && users.length === 0) {
    return (
      <ErrorView
        error={error}
        onRetry={refetch}
      />
    );
  }

  return (
    <SafeAreaView style={styles.container} edges={['bottom']}>
      <FlatList
        data={users}
        renderItem={renderItem}
        keyExtractor={keyExtractor}
        refreshControl={
          <RefreshControl
            refreshing={isRefreshing}
            onRefresh={refetch}
          />
        }
        contentContainerStyle={styles.listContent}
        ItemSeparatorComponent={Separator}
        removeClippedSubviews
        maxToRenderPerBatch={10}
        windowSize={5}
      />
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
  },
  centered: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  listContent: {
    padding: 16,
  },
});
```

### Phase 5: Mobile Performance Optimization

```markdown
## Mobile Performance Checklist

### Launch Time Optimization
- [ ] Cold start < 1 second (target)
- [ ] Defer non-critical initialization
- [ ] Lazy load modules/screens
- [ ] Optimize main thread work
- [ ] Profile with Instruments (iOS) / Profiler (Android)

### Memory Management
- [ ] Monitor memory usage in profiler
- [ ] Release unused resources
- [ ] Use weak references appropriately
- [ ] Implement proper image caching
- [ ] Handle low memory warnings

### Network Optimization
- [ ] Implement request caching
- [ ] Use compression (gzip, brotli)
- [ ] Batch API requests where possible
- [ ] Implement offline-first architecture
- [ ] Use WebSocket for real-time features

### UI Performance
- [ ] 60 FPS during scrolling
- [ ] Avoid layout thrashing
- [ ] Use proper list virtualization
- [ ] Optimize image loading (lazy, proper sizing)
- [ ] Minimize re-renders (React Native)

### Battery Optimization
- [ ] Minimize background processing
- [ ] Batch network requests
- [ ] Use efficient location updates
- [ ] Optimize push notification handling
- [ ] Reduce GPS/sensor polling frequency
```

---

## Expert Decision Frameworks

### Architecture Pattern Selection

| Pattern | Complexity | Testability | Scalability | Best For |
|---------|------------|-------------|-------------|----------|
| **MVC** | Low | ⭐⭐ | ⭐⭐ | Simple apps, prototypes |
| **MVP** | Medium | ⭐⭐⭐⭐ | ⭐⭐⭐ | Legacy Android |
| **MVVM** | Medium | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | SwiftUI, Compose, most apps |
| **MVI** | High | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | Complex state, Redux-style |
| **Clean** | High | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | Enterprise, long-lived apps |
| **VIPER** | Very High | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | Large teams, modular apps |

### State Management Selection

| Solution | Platform | Learning Curve | Performance | Use Case |
|----------|----------|----------------|-------------|----------|
| **@State/@Published** | iOS | Low | Excellent | Simple local state |
| **StateFlow** | Android | Medium | Excellent | Reactive state |
| **Redux** | Cross-platform | High | Good | Complex global state |
| **Zustand** | React Native | Low | Excellent | Simple global state |
| **Riverpod** | Flutter | Medium | Excellent | Dependency injection + state |

---

## Anti-Patterns to Avoid

| Anti-Pattern | Problem | Better Approach |
|--------------|---------|-----------------|
| **Massive View Controllers** | Untestable, unmaintainable | Extract logic to ViewModels/UseCases |
| **Direct API calls in UI** | Coupling, hard to test | Repository pattern with dependency injection |
| **Ignoring lifecycle** | Memory leaks, crashes | Proper lifecycle-aware components |
| **No offline support** | Poor UX on flaky connections | Offline-first architecture |
| **Blocking main thread** | ANR/frozen UI | Background threads for heavy work |
| **Hardcoded strings** | Localization nightmare | Resource files for all strings |
| **No error handling** | Crashes, poor UX | Graceful error states and recovery |
| **Missing analytics** | Blind to user behavior | Comprehensive event tracking |

---

## App Store Submission Checklist

```markdown
## iOS App Store
- [ ] App icons (all sizes)
- [ ] Launch screen
- [ ] Privacy policy URL
- [ ] App Store screenshots (all device sizes)
- [ ] App description and keywords
- [ ] Export compliance (encryption)
- [ ] Sign in with Apple (if social login)
- [ ] In-app purchase testing
- [ ] Push notification entitlements
- [ ] App Transport Security compliance

## Google Play Store
- [ ] App icons (512x512)
- [ ] Feature graphic (1024x500)
- [ ] Screenshots (phone, tablet)
- [ ] Store listing (short/full description)
- [ ] Content rating questionnaire
- [ ] Data safety form
- [ ] Target API level compliance
- [ ] 64-bit support
- [ ] Privacy policy URL
- [ ] App signing by Google Play
```

---

## Quick Reference

```bash
# iOS
xcodebuild -scheme MyApp -configuration Release archive
xcrun altool --upload-app -f MyApp.ipa -u user@email.com
pod install
swift package resolve

# Android
./gradlew assembleRelease
./gradlew bundleRelease
adb install app-release.apk
adb logcat *:E

# React Native
npx react-native run-ios --configuration Release
npx react-native run-android --variant=release
npx react-native start --reset-cache

# Flutter
flutter run --release
flutter build appbundle
flutter build ipa
flutter doctor
```

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-01-13 | Initial expert skill creation |
