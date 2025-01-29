import { useState, useEffect } from "react";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";

export default function GitHubProfile() {
  const [username, setUsername] = useState("octocat");
  const [repos, setRepos] = useState([]);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    fetchRepos(username);
  }, []);

  const fetchRepos = async (user) => {
    setLoading(true);
    try {
      const response = await fetch(`https://api.github.com/users/${user}/repos`);
      if (!response.ok) throw new Error("User not found");
      const data = await response.json();
      setRepos(data);
    } catch (error) {
      console.error(error);
      setRepos([]);
    }
    setLoading(false);
  };

  return (
    <div className="max-w-2xl mx-auto p-4 text-center">
      <h1 className="text-2xl font-bold mb-4">GitHub Repositories</h1>
      <div className="flex gap-2 mb-4">
        <Input
          type="text"
          value={username}
          onChange={(e) => setUsername(e.target.value)}
          placeholder="Enter GitHub username"
        />
        <Button onClick={() => fetchRepos(username)}>Search</Button>
      </div>
      {loading ? (
        <p>Loading...</p>
      ) : (
        <div className="grid gap-4">
          {repos.map((repo) => (
            <Card key={repo.id}>
              <CardContent className="p-4">
                <a href={repo.html_url} target="_blank" rel="noopener noreferrer" className="text-blue-500 font-semibold">{repo.name}</a>
                <p className="text-sm text-gray-600">{repo.description || "No description"}</p>
              </CardContent>
            </Card>
          ))}
        </div>
      )}
    </div>
  );
}
