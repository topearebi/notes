# 📚 Resources Hub

## 📖 Books & Reading
${table.concat(query[[
  from p = index.tag "page"
  where string.find(p.name, "^resources/booknotes/")
  order by p.lastModified desc
  limit 5
  select "* [[" .. p.name .. "]]"
]], "\n")}

## 🗄️ All Resources
${table.concat(query[[
  from p = index.tag "page"
  where string.find(p.name, "^resources/") and p.name ~= "resources/index" and p.name ~= "resources/"
  order by p.name
  select "* [[" .. p.name .. "]]"
]], "\n")}
