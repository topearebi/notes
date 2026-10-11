# 🚀 Projects Hub

## 🏗️ Active Projects
${table.concat(query[[
  from p = index.tag "page"
  where string.find(p.name, "^projects/") and p.name ~= "projects/index" and p.name ~= "projects/"
  order by p.lastModified desc
  select "* [[" .. p.name .. "]]"
]], "\n")}

## 📂 Categories
* [[projects/certifications]]
* [[projects/writing]]
* [[projects/languages]]
* 