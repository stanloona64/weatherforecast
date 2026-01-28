# 1. AŞAMA: Build (Derleme)
# SDK imajını kullanıyoruz çünkü derleme araçlarına ihtiyacımız var.
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app

# Bağımlılıkları (nuget paketleri) kopyala ve restore et. 
# Bu adımı ayrı yapıyoruz ki kod her değiştiğinde paketleri tekrar indirmesin (Layer Caching).
COPY *.csproj ./
RUN dotnet restore

# Kalan tüm dosyaları kopyala ve uygulamayı derle
COPY . ./
RUN dotnet publish -c Release -o out

# 2. AŞAMA: Runtime (Çalışma Zamanı)
# Sadece çalışması için gereken hafif (alpine veya slim) imajı kullanıyoruz.
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS runtime
WORKDIR /app

# Derleme aşamasından sadece 'out' klasörünü alıyoruz (Multi-stage magic!)
COPY --from=build /app/out .

# Güvenlik: Uygulamayı root yetkisi olmayan bir kullanıcıyla çalıştırmak best-practice'dir.
USER 1000

ENTRYPOINT ["dotnet", "MyTestApp.dll"]